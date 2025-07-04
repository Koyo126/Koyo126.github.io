---
layout: post
title:  "git cherry-pick の使い方"
date:   2025-06-26
---

## `git cherry-pick` とは

他のブランチにある**特定のコミットだけ**を選択し、現在のブランチに新しいコミットとしてコピーするコマンドである。ブランチ全体の変更を取り込む`merge`や`rebase`とは異なり、コミット単位で柔軟に変更を適用できる。

### 主な利用シーン

-   **先行リリース**: 機能ブランチにある特定のバグ修正だけを、先に`main`ブランチへ反映させる。
-   **コミット先の修正**: 間違ったブランチにしたコミットを、本来あるべきブランチへ移動させる。
-   **複数ブランチへの適用**: ある重要な修正を、他の複数の開発ブランチにも適用する。

### 基本的な使い方

1.  **適用先のブランチに移動**
    コミットを適用したい先のブランチ（例: `main`）へ移動する。ここが操作の視点となる。
    ```sh
    git switch main
    ```

2.  **コミットハッシュを確認**
    `git log <コミット元ブランチ>`で、取り込みたいコミットのハッシュ値（例: `a1b2c3d`）を調べる。
    ```sh
    git log feature/new-login --oneline
    ```

3.  **コマンドを実行**
    移動後のブランチ（この例では `main`）上で、対象のコミットを指定してコマンドを実行する。
    ```sh
    git cherry-pick a1b2c3d
    ```

### 別ディレクトリのリポジトリから適用する方法

1.  **変更元をリモートとして追加**
    適用先のリポジトリ内で作業する。
    ```sh
    git remote add temp-source /path/to/source-repo
    ```
2.  **変更元リポジトリの情報を取得**
    ```sh
    git fetch temp-source
    ```
3.  **`cherry-pick`を実行**
    適用先のブランチ上で実行する。
    ```sh
    git cherry-pick <適用したいコミットのハッシュ値>
    ```
4.  **後片付け**
    ```sh
    git remote remove temp-source
    ```
