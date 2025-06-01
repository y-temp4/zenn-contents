---
publication_name: "typebase_dev"
title: "AIを活用してTypeScriptのpath aliasを相対パスへ一括変換するスクリプトを生成"
emoji: "🔁"
type: "tech"
topics: ["typescript", "ai", "chatgpt", "tsmorph", "contest2025ts"]
published: true
---

TypeScript のプロジェクトで path alias を使って `@/components/Button` のような書き方をしていましたが、プロジェクトの都合により path alias を廃止し相対パスへ統一することになりました[^path-alias]。

[^path-alias]: path alias を使わないことによる利点については [azu さんの gist](https://gist.github.com/azu/56a0411d69e2fc333d545bfe57933d07#paths) が参考になります。

しかし大量の import 文を手動で変換することは現実的でありません。そこで AI を活用して変換スクリプトを生成し、一括で変換した事例を紹介します。

```typescript
// 変換前
import { Button } from "@/components/ui/button"
import { useAuth } from "@/hooks/useAuth"

// 変換後
import { Button } from "../../components/ui/button"
import { useAuth } from "../hooks/useAuth"
```

## AIの利用手順

### 🔍 事前調査

いきなり「変換スクリプトを作って」と依頼するのではなく、まずは **適切な解決方法を調査** することから始めました。

```
tsconfigのpath aliasをなくしたいです。

"paths": { 
  "@/*": ["./src/*"] 
} 

一括で相対パス（例："../../utils"）に変換する方法があれば教えて下さい。
```


### 💡 調査結果

AI から以下のような回答を得ました。

1. [tsc-alias](https://github.com/justkey007/tsc-alias) を利用する
2. AST 変換ツール（例: [jscodeshift](https://jscodeshift.com/) や [ts-morph](https://ts-morph.com/)）を利用する

tsc-alias はビルド時にエイリアスを解決するツールであり、ソースコード自体を物理的に相対パスに書き換えるものではないため、今回の目的には合致しませんでした。

そこで、コードの抽象構文木（AST）を操作し、直接コードを書き換えることができる ts-morph を利用することにしました。

### 🤖 スクリプト生成の依頼

プロンプトは以下のようにシンプルなものにしました。

```
ts-morph を使う方法を教えて下さい。
```
## 生成されたスクリプト

最初に AI が生成したスクリプトがこちらです。

```typescript
import { Project } from "ts-morph";
import * as path from "path";

// tsconfig.json のパスとエイリアスのマッピング情報
const tsConfigFilePath = "./tsconfig.json";
const alias = "@/"; // tsconfig で定義しているエイリアス
const aliasTarget = "src/"; // エイリアスが指す実際のパス（例："./src"）

// プロジェクトの読み込み
const project = new Project({
  tsConfigFilePath,
});

const sourceFiles = project.getSourceFiles();

// 各ソースファイルについて import 文をチェック
sourceFiles.forEach(sourceFile => {
  const importDeclarations = sourceFile.getImportDeclarations();
  importDeclarations.forEach(importDecl => {
    const moduleSpecifierValue = importDecl.getModuleSpecifierValue();

    // エイリアスが使われている場合のみ処理
    if (moduleSpecifierValue.startsWith(alias)) {
      // エイリアス部分を置換して、ターゲットパスを作成
      const withoutAlias = moduleSpecifierValue.replace(alias, aliasTarget);
      // ソースファイルのディレクトリから見た絶対パス
      const sourceDir = sourceFile.getDirectoryPath();
      const absoluteTargetPath = path.resolve(sourceDir, withoutAlias);
      // 現在のファイルからターゲットへの相対パスを計算
      let relativePath = path.relative(sourceDir, absoluteTargetPath);

      // OS によってはパス区切りが \ になっているため、/ に変換
      relativePath = relativePath.split(path.sep).join("/");

      // 相対パスの先頭がドットで始まらない場合は補正
      if (!relativePath.startsWith(".")) {
        relativePath = "./" + relativePath;
      }

      // import 文を書き換え
      importDecl.setModuleSpecifier(relativePath);
      console.log(`Changed ${moduleSpecifierValue} -> ${relativePath} in ${sourceFile.getFilePath()}`);
    }
  });
});

// 変更を保存
project.saveSync();
```

一発目でわりといい感じのスクリプトが生成されました。

ただし、これを実行するとうまくいかない箇所があったため、何度か修正を繰り返すことに。
修正を出す際は、具体的なコード例を出して指示することを心がけました。

最終的には以下のようなスクリプトになりました（手動での修正も含む）。

```typescript:convert-alias.ts
// @ts-ignore
import * as path from "node:path";
import { Project } from "ts-morph";

const tsConfigFilePath = "./tsconfig.json";
const alias = "@/"; // tsconfig.json で定義されているエイリアス
const aliasTarget = "src/"; // エイリアスの実際のパス（例: "./src/"）

// プロジェクトルートを取得（tsconfig.json のあるディレクトリ）
const projectRoot = path.resolve(path.dirname(tsConfigFilePath));

const project = new Project({
  tsConfigFilePath,
});

const sourceFiles = project.getSourceFiles();

sourceFiles.forEach((sourceFile) => {
  const importDeclarations = sourceFile.getImportDeclarations();

  importDeclarations.forEach((importDecl) => {
    const moduleSpecifierValue = importDecl.getModuleSpecifierValue();

    // エイリアスを使用している import 文のみ処理
    if (moduleSpecifierValue.startsWith(alias)) {
      // エイリアス部分を除いたパス（例: "components/ui/button"）
      const aliasSuffix = moduleSpecifierValue.slice(alias.length);

      // 絶対パスを計算（例: /Users/.../src/components/ui/button
      const absoluteTargetPath = path.resolve(projectRoot, aliasTarget, aliasSuffix);

      // 現在のファイルのディレクトリを取得（例: /Users/.../src/components）
      const sourceDir = sourceFile.getDirectoryPath();

      // 元のファイル位置からターゲットへの相対パスを計算（例: "../../../components/ui/button"）
      let relativePath = path.relative(sourceDir, absoluteTargetPath);

      // Windows 環境対応（\ を / に統一）
      relativePath = relativePath.replace(/\\/g, "/");

      // 相対パスの先頭が `.` で始まるように調整
      if (!relativePath.startsWith(".")) {
        relativePath = `./${relativePath}`;
      }

      // import 文を書き換え
      importDecl.setModuleSpecifier(relativePath);
      console.log(`Updated: ${moduleSpecifierValue} -> ${relativePath} in ${sourceFile.getFilePath()}`);
    }
  });
});

// 変更を保存
project.saveSync();
```

生成された移行スクリプトは [tsx](https://tsx.is/) で実行しました。

```bash
tsx ./convert-alias.ts
```

## 実行結果

変換したファイル数は 200 件を超えていましたが、最終的に型チェック・テストすべて通過する形で移行することができました。

しかも、直接 import 文に関連する箇所において手動修正が必要だった箇所は 0 件でした✨[^vi-mock]

[^vi-mock]: ただし唯一 `vi.mock` の第一引数のパス指定において、AI によるスクリプトでは適切に変換できず、手動でのパス修正が必要でした。

影響範囲が広い変更でしたが、AI を活用することで効率的に変換を行うことができました。

## おわりに

いきなり実装を依頼するのではなく、その前に準備段階として実行計画を立てることが重要だと改めて認識しました✨

これからも AI を活用して、より効率よく開発を進めていきたいと思います。
