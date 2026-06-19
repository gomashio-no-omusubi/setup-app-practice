# setup-app-practice

## 概要

COACHTECH教材のLaravel SailおよびDockerを用いた開発環境構築のハンズオンリポジトリです。
環境構築の自動化ツールの利便性を体感するとともに、実務を想定した環境構築手法の選定や構造の理解を目的に作成しました。

## 使用技術

- PHP 8.5.1
- Laravel 10.x
- Laravel Sail（Docker Compose）
- MySQL
- phpMyAdmin

## 学んだこと

- **Laravel Sailによる環境構築の効率化**：
  DockerfileやNginxの設定ファイル、Docker Composeの環境変数を手動で記述・修正することなく、コマンド一発で整合性の取れた開発環境が自動生成される利便性を学びました。
- **実務における開発環境の判別手法（Gitクローン時）**：
  Gitクローンした既存プロジェクトにおいて、手動構築環境かSail環境かを判別する手法を習得しました。
    1. `README.md` に記載されている起動コマンドの確認
    2. プロジェクトのファイル構成の確認（`Dockerfile` の有無、または `compose.yaml` 内のサービス名が `laravel.test` かどうかの確認）
- **Laravel Sailのアーキテクチャと遠隔操作の仕組み**：
  手動構築のように `docker-compose exec php bash` で毎回コンテナ内にログインする必要がなく、ローカルのターミナルから `sail` コマンド（自動化スクリプト）を通じて遠隔で安全にコンテナ内を操作できる仕組みを理解しました。

## セットアップ手順

任意のディレクトリでリポジトリをクローンし、依存パッケージのインストールから開発環境（コンテナ）の起動までを行います。

_※他ユーザーのローカルPC環境への配慮（環境の書き換え防止）のため、エイリアス未設定の状態でも確実に動作するフルパス形式で記述しています。_

### 1. リポジトリをクローン

```bash
git clone git@github.com:gomashio-no-omusubi/setup-app-practice.git
```

### 2. 作成したディレクトリに移動

```bash
cd setup-app-practice
```

### 3. 環境設定ファイルの作成

```bash
cp .env.example .env
```

### 4. 依存パッケージ（Laravel Sail等）の一括インストール

```bash
docker run --rm \
    -u "$(id -u):$(id -g)" \
    -v "$(pwd):/var/www/html" \
    -w /var/www/html \
    laravelsail/php85-composer:latest \
    composer install
```

### 5. Dockerコンテナの起動

```bash
./vendor/bin/sail up -d
```

_※ コンテナが完全に起動するまで、スペックにより数十秒〜数分かかる場合があります。_

### 6. アプリケーションキーの生成

```bash
./vendor/bin/sail artisan key:generate
```

### 7. マイグレーションとシーディングの実行

```bash
./vendor/bin/sail artisan migrate --seed
```

### 8. ブラウザでのアクセス確認

コンテナ起動後、ブラウザで以下のURLにアクセスし、Laravelの初期画面が表示されることを確認してください。

- **Laravel アプリケーション**：http://localhost
