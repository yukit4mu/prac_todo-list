# setup_docker-laravel

### セットアップ手順

### 1. docker template をクローン

    git clone git@github.com:coachtech-material/laravel-docker-template.git`

### 2. ローカルリポジトリから紐付け先を変更

    1. `git remote set-url origin 作成したリポジトリのurl`
        1. 現在のリポジトリのリモートURLを新しいリモートURLに変更する
    2. `git remote -v`
        1. 現在のリモートリポジトリへのurlを確認できる
    3. add, commit, pushを実行
        1. commitの際に”リモートリポジトリの変更” と入れる

### 3. docker コンテナを立ち上げる

    1. `docker-compose up -d --build`
        **`docker-compose up -d --build`**コマンドは、Composeファイルに定義されたサービスをバックグラウンドで起動し、必要な場合にDockerイメージを再ビルドします。
        1. **`docker-compose`**: Docker Composeは、複数のDockerコンテナを定義し、管理するためのツールです。Docker Composeファイル(**`docker-compose.yml`**)に定義されたサービスとコンテナを操作します。
        2. **`up`**: このコマンドは、Composeファイルに定義されたサービスを起動します。つまり、Composeファイルに記述されたコンテナを作成および起動します。
        3. **`d`**または**`-detach`**: このフラグは、コンテナをバックグラウンドで実行するために使用されます。つまり、コンテナが起動すると、コマンドプロンプト(ターミナル)がブロックされずに他の作業を行えます。
        4. **`-build`**: このフラグは、Composeファイルに定義されたサービスのDockerイメージをビルドする必要があることを示します。Composeファイルにはサービスがビルドされる方法が定義されている場合、このフラグを指定することでイメージの再ビルドが行われます。ビルドは、Dockerfileを使用してコンテナイメージを作成するプロセスです。

### 4. PHP コンテナ内にログイン

    1. `docker-compose exec php bash`
        **`docker-compose exec php bash`**は、Docker Composeを使用して実行中のコンテナ内のシェルセッションを起動するコマンドです。
        1. **`exec`**: このコマンドは、Dockerコンテナ内でコマンドを実行するために使用されます。**`docker-compose exec`**を使用することで、Composeファイルに定義されたサービス内のコンテナにアクセスできます。
        2. **`php`**: これはComposeファイル内のサービス名です。具体的なサービス名はComposeファイルに定義されており、通常、アプリケーション内の特定のコンポーネントを表します。このコマンドは、**`php`**というサービス内のコンテナにアクセスしようとしています。
        3. **`bash`**: これはコンテナ内で起動するシェルのコマンドです。具体的には、Bashシェルを起動し、コンテナ内で対話的なシェルセッションを開始します。これにより、コンテナ内でコマンドを実行したり、ファイルを操作したりできます。
    2. `composer install`
        PHPコンテナ内でComposerが起動され、プロジェクトの依存関係が解決され、必要なライブラリやパッケージがプロジェクトにインストールされます。
        1. **`composer install`**コマンドは、Composerの設定ファイルである**`composer.json`**を参照し、プロジェクトに記述されている依存関係パッケージをインストールします。これにより、プロジェクトの依存関係が解決され、必要なパッケージがダウンロードされ、インストールされます。
    3. `cp .env.example .env`
        .env.exampleファイルを.envに変更する。

### 5. `.env`ファイル内の記述を変更する

    docker-compose.yml内のmysqlの表記と合わせる
    ```php
    DB_CONNECTION=mysql
    - DB_HOST=127.0.0.1
    + DB_HOST=mysql
    DB_PORT=3306
    - DB_DATABASE=laravel
    - DB_USERNAME=root
    - DB_PASSWORD=
    + DB_DATABASE=laravel_db
    + DB_USERNAME=laravel_user
    + DB_PASSWORD=laravel_pass
    ```

### 6. アプリケーションキーを生成

    アプリケーションキーは暗号化の際に利用されるランダムな英数字32文字です。
    ※ 変更されると復号化に影響が出るので、運用(キーが設置された)後には実行NG
    1. phpコンテナ内で`php artisan key:generate` を実行

## 補足
    これをクローンして、"git remote set-url origin　新プロジェクトリポURL"しちゃえばいい。次から。
    あと、外部からクローンしてきたものはマイグレーションとシーディング必要だからね。
