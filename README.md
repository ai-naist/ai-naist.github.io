# ai-naist.github.io

# [Jekyll + Github Pages で Blog] ローカル開発環境構築ガイド (Windows + VS Code + uru)

このドキュメントは、[Jekyll + Github Pages で Blog] を Windows 11 環境下の Visual Studio Code (VS Code) で、uru と Bundler を使用してローカル開発を行うための手順をまとめたものです。

## 1. 前提となる環境とツール

以下のツールがインストールされ、設定されていることを前提とします。

*   **Windows 11**
*   **Visual Studio Code (VS Code):** [公式サイト](https://code.visualstudio.com/)
*   **Git for Windows:** [公式サイト](https://gitforwindows.org/)
*   **Ruby (複数バージョン):**
    *   [RubyInstaller for Windows](https://rubyinstaller.org/downloads/) を使用し、**WITH DEVKIT 版** を **異なるディレクトリ** (例: `C:\Ruby31`, `C:\Ruby30`) にインストールしてください。
    *   **重要:** インストール時に「Add Ruby executables to your PATH」のチェックは **外して** ください。
    *   各バージョンで `ridk install` を実行して MSYS2 development toolchain をセットアップしてください。
*   **uru:**
    *   [uru リリースページ](https://bitbucket.org/jonforums/uru/downloads/) から `uru_rt_*.exe` をダウンロードし、`uru.exe` にリネームします。
    *   `uru.exe` を PATH が通っているディレクトリ (例: `C:\Users\[あなたのユーザー名]\bin` など、ご自身で決めた場所) に配置します。
    *   配置したディレクトリが **システムの PATH 環境変数に追加されている** ことを確認してください。（追加後は PowerShell の再起動が必要です）
    *   **管理者権限の PowerShell** で `uru_rt admin install` を実行し、その後 PowerShell を再起動します。

## 2. 初期セットアップ (初回のみ)

1.  **リポジトリのクローン:**
    *   まだクローンしていない場合は、任意の作業ディレクトリで実行します。
    ```bash
    git clone https://github.com/[あなたのユーザー名]/[リポジトリ名].git
    cd [リポジトリ名]
    ```
2.  **VS Code でプロジェクトを開く:**
    *   プロジェクトのルートディレクトリで `code .` コマンドを実行するか、VS Code のメニューからフォルダを開きます。
3.  **uru に Ruby 環境を登録:**
    *   PC 全体で一度だけ行えば良い作業です。インストールした各 Ruby 環境を `uru` に登録します。タグ名 (`ruby31` など) は自由に決められます。
        ```powershell
        # 通常の PowerShell で実行
        uru admin add C:\Ruby31 --tag ruby31
        uru admin add C:\Ruby30 --tag ruby30
        # 登録されたか確認
        uru ls
        ```
4.  **Bundler 設定 (Gem の隔離):**
    *   **VS Code の統合ターミナル** ([ターミナル] > [新しいターミナル]) を開きます。
    *   以下のコマンドを実行し、Gem のインストール先をプロジェクト配下の `vendor/bundle` に設定します。**これはプロジェクトごとに初回のみ実行します。**
        ```powershell
        bundle config set --local path 'vendor/bundle'
        ```
5.  **依存関係のインストール:**
    *   まず、このプロジェクトで使用する Ruby バージョンを選択します。(例: `ruby31` タグを指定)
        ```powershell
        uru ruby31
        ```
    *   次に、`Gemfile` に基づいて必要な Gem をインストールします。(`Gemfile` が存在しない場合は、下記の「4. `Gemfile` と `.gitignore`」を参照して作成してください。)
        ```powershell
        bundle install
        ```
    *   `.gitignore` ファイルが存在し、適切に設定されているか確認してください。(下記の「4. `Gemfile` と `.gitignore`」参照)

## 3. 日常的な開発フロー

1.  **VS Code でプロジェクトを開き、統合ターミナルを開きます。**
2.  **使用する Ruby バージョンを選択:**
    *   **ターミナルを開くたびに**、このプロジェクトで使用したい Ruby のタグを指定します。(例)
        ```powershell
        uru ruby31
        ```
    *   `ruby -v` でバージョンを確認できます。
3.  **ローカルサーバーを起動:**
    *   Jekyll サイトをプレビューするためのサーバーを起動します。`--livereload` オプションを付けると、ファイル保存時にブラウザが自動でリロードされます。
        ```powershell
        bundle exec jekyll serve --livereload
        ```
4.  **ブラウザでプレビュー:**
    *   ウェブブラウザを開き、`http://127.0.0.1:4000/` (またはターミナルに表示されるアドレス) にアクセスします。
5.  **コードの編集:**
    *   VS Code でプロジェクト内のファイル (`.md`, `_layouts/*.html`, `_includes/*.html`, CSS など) を編集・保存します。
    *   `--livereload` が有効な場合、保存するとブラウザに変更が自動で反映されます。
6.  **開発終了:**
    *   ローカルサーバーを停止するには、サーバーが起動しているターミナルで `Ctrl + C` を押します。
7.  **変更のコミットとプッシュ:**
    *   編集した内容を Git で保存し、GitHub リポジトリに反映させます。
        ```bash
        # 変更されたファイルをステージング
        git add .
        # 変更内容をコミット
        git commit -m "ここに具体的な変更内容のメッセージを書く"
        # GitHub にプッシュ (main はデフォルトブランチ名に合わせて変更)
        git push origin main
        ```

## 4. `Gemfile` と `.gitignore`

これらのファイルはプロジェクトのルートディレクトリに配置します。

*   **`Gemfile`:** プロジェクトが依存する Gem (ライブラリ) を定義します。GitHub Pages で Jekyll を使う場合、以下が基本的な内容です。
    ```ruby
    # Gemfile
    source "https://rubygems.org"

    # GitHub Pages 環境との互換性を保つための Gem
    gem "github-pages", group: :jekyll_plugins

    # 必要に応じて他の Gem を追加
    # gem "jekyll-seo-tag"
    ```
*   **`.gitignore`:** Git のバージョン管理から除外するファイルやディレクトリを指定します。Jekyll プロジェクトでは以下を含めるのが一般的です。
    ```gitignore
    # Jekyll build output
    _site/

    # Bundler path (プロジェクト内に Gem をインストールする場合)
    vendor/bundle/
    .bundle/

    # Jekyll cache files
    .jekyll-cache/
    .jekyll-metadata

    # Sass cache
    .sass-cache/

    # OS generated files
    .DS_Store
    Thumbs.db
    ```

## 5. トラブルシューティング

*   **`uru` コマンドが見つからない (`CommandNotFoundException`):**
    *   `uru.exe` を配置したディレクトリがシステムの PATH 環境変数に正しく設定されているか確認してください。
    *   環境変数を変更した後は、必ず PowerShell (または VS Code) を再起動してください。
    *   `uru_rt admin install` (管理者権限で実行) が完了しているか確認してください。
*   **`Could not locate Gemfile`:**
    *   現在のディレクトリがプロジェクトのルートディレクトリであり、そこに `Gemfile` という名前のファイルが存在するか確認してください。
*   **Jekyll コマンド実行時にエラーが出る:**
    *   `uru <タグ名>` で正しい Ruby バージョンを選択しているか確認してください。
    *   `bundle install` を実行して、必要な Gem が正しくインストールされているか確認してください。エラーメッセージをよく読んでみてください。
    *   コマンドの前に `bundle exec` を付け忘れていないか確認してください (例: `bundle exec jekyll serve`)。

---

https://realfavicongenerator.net/
