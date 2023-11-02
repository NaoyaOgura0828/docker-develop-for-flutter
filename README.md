# docker-develop-for-flutter
[Docker](https://www.docker.com/)で[Flutter](https://flutter.dev/)開発環境を構築する。

<br>

# Requirement
以下のlocalhost環境で動作確認済み<br>
- [Fedora](https://fedoraproject.org/ja/)38
- [Windows](https://www.microsoft.com/ja-jp/windows/)10

<br>

Hostに[VSCode](https://code.visualstudio.com/Download)と以下のVSCode拡張機能をInstallする。
- [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)
- [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)

Hostに[Android Studio](https://developer.android.com/studio/install?hl=ja)をInstallする。
> **Note**<br>
> [Android Studio](https://developer.android.com/studio/install?hl=ja)のInstallは、[JetBrains Toolbox App](https://www.jetbrains.com/ja-jp/toolbox-app/)を利用したInstallを推奨する。

<br>

# Installation
git cloneコマンドで本Repositoryを任意のディレクトリ配下にcloneする。

<br>

# Settings
[.env](./.env)を設定することで、任意の設定でContainerを実行する事が可能である。

## 実行ユーザー名の設定
[.env](./.env)内の`USER_NAME`にコンテナ起動後の実行ユーザーを設定する。

```
USER_NAME = ${実行ユーザー名}
```

<br>

## コンテナイメージ名の設定
[.env](./.env)内の`IMAGE_NAME`を任意のコンテナイメージ名に設定する。

```
IMAGE_NAME = ${コンテナイメージ名}
```

<br>

> **Warning**<br>
> コンテナイメージは以下の命名規則に従うこと。<br>
> `^[a-z0-9][a-z0-9_.-]{1,}$`

<br>

> **Note**<br>
> [DockerHub](https://hub.docker.com/)へコンテナイメージのPUSHを想定する場合は以下の命名規則に従うこと。
> ```
> IMAGE_NAME = ${DockerHubユーザー名}/${コンテナイメージ名}:${タグ名}
> ```

<br>

## コンテナ名の設定(Optional)
[.env](./.env)内の`CONTAINER_NAME`を任意のコンテナ名に設定する。
<br>
コンテナ名が起動中のコンテナと重複しないように留意する。

```
CONTAINER_NAME = ${コンテナ名}
```

<br>

## ボリューム名の設定(Optional)
[.env](./.env)内の`VOLUME_NAME`を任意のボリューム名に設定する。
<br>
ボリューム名が起動中のコンテナと重複しないように留意する。

```
VOLUME_NAME = ${ボリューム名}
```

<br>

## ネットワーク名の設定(Optional)
[.env](./.env)内の`NETWORK_NAME`を任意のネットワーク名に設定する。
<br>
ネットワーク名が起動中のコンテナと重複しないように留意する。

```
NETWORK_NAME = ${ネットワーク名}
```

<br>

## Flutter Versionの設定(Optional)
[.env](./.env)内の`FLUTTER_VERSION`を任意の[Flutter Version](https://docs.flutter.dev/release/archive?tab=linux)に設定する。

```
FLUTTER_VERSION = ${Flutter Version}
```

<br>

## commandlinetools Versionの設定(Optional)
[.env](./.env)内の`ANDROID_COMMAND_LINE_TOOLS_VERSION`を任意の[commandlinetools Version](https://developer.android.com/studio#command-line-tools-only)に設定する。

```
ANDROID_COMMAND_LINE_TOOLS_VERSION = ${commandlinetools Version}
```

> **Note**<br>
> `linux`Platformを設定する事。<br>
> ex.<br>
> commandlinetools-linux-10406996_latest.zip

<br>

## Android API Levelの設定(Optional)
[.env](./.env)内の`ANDROID_API_LEVEL`を任意の[Android API Level](https://developer.android.com/guide/topics/manifest/uses-sdk-element?hl=ja#ApiLevels)に設定する。

```
ANDROID_API_LEVEL = ${Android API Level}
```

<br>

## Build Tools Versionの設定(Optional)
[.env](./.env)内の`ANDROID_SDK_BUILD_TOOLS_VERSION`を任意の[Build Tools Version](https://developer.android.com/tools/releases/build-tools?hl=ja)に設定する。

```
ANDROID_SDK_BUILD_TOOLS_VERSION = ${Build Tools Version}
```

<br>

## HostがLinux以外の場合(Optional)
HostがLinux以外の場合は、[docker-compose.yml](./docker-compose.yml)内の`Valid only if the host OS is Linux`とコメントされている行をコメントアウトする。

```yml
    volumes:
      - /etc/passwd:/etc/passwd:ro # Valid only if the host OS is Linux
      - /etc/group:/etc/group:ro # Valid only if the host OS is Linux
```

<br>

# Usage

## コンテナ実行
本Repository直下([docker-compose.yml](./docker-compose.yml)が存在するディレクトリ)で以下のコマンドを実行する。

```bash
docker compose up -d --build
```

<br>

## コンテナ環境へのアクセス
1. VSCodeの拡張機能左メニューから拡張機能`リモートエクスプローラー`を押下する。

<img src='images/RemoteDevelopment_RemoteExplorer.png'>

<br>

2. プルダウンを`開発コンテナー`に変更し、コンテナ一覧から本リポジトリ名にマウスオーバーする。

<img src='images/RemoteDevelopment_DevContainer.png'>

<br>

3. 右側に表示される`新しいウィンドウでアタッチする`を押下する。

<img src='images/RemoteDevelopment_AttachNewWindow.png'>

<br>

# Demo
## Project 作成
Container環境で下記コマンドを実行する。

```bash
$ flutter create testapp
```

> **Note**<br>
> Demoでは`testapp`としているが、Application名は任意の名称で良い。

<br>

## Application 実行
### Browser 実行
1. 実行<br>
Container環境で下記コマンドを実行する。

```bash
$ cd testapp
$ flutter run -d web-server

.Launching lib/main.dart on Web Server in debug mode...
Waiting for connection from debug service on Web Server...         18.9s
lib/main.dart is being served at http://localhost:33113
The web-server device requires the Dart Debug Chrome extension for debugging. Consider using the Chrome or Edge devices for an improved development workflow.

🔥  To hot restart changes while running, press "r" or "R".
For a more detailed help message, press "h". To quit, press "q".
```

2. 動作確認<br>
HostのBrowserでhttp://localhost:33113へアクセスし、動作確認を実施する。

> **Note**<br>
> http://localhost:33113のport番号はランダムである。

<br>

### AndroidEmulator 実行
1. 接続<br>
Hostで[AndroidEmulator](https://developer.android.com/studio/run/managing-avds?hl=ja)を起動し、Container環境で下記コマンドを実行する。

```bash
$ adb connect host.docker.internal:5555

* daemon not running; starting now at tcp:5037
* daemon started successfully
connected to host.docker.internal:5555
```

2. 実行<br>
Container環境で下記コマンドを実行する。

```bash
$ cd testapp
$ flutter run -d host.docker.internal:5555
Using hardware rendering with device sdk gphone64 x86 64. If you notice graphics artifacts, consider enabling software rendering with "--enable-software-rendering".
Launching lib/main.dart on sdk gphone64 x86 64 in debug mode...
Running Gradle task 'assembleDebug'...                              5.5s
✓  Built build/app/outputs/flutter-apk/app-debug.apk.
Installing build/app/outputs/flutter-apk/app-debug.apk...           4.2s
Error: ADB exited with exit code 1
Performing Streamed Install

adb: failed to install /home/satoasu/testapp/build/app/outputs/flutter-apk/app-debug.apk: Failure [INSTALL_FAILED_UPDATE_INCOMPATIBLE: Package com.example.testapp signatures do not match previously installed version; ignoring!]
Uninstalling old version...
Installing build/app/outputs/flutter-apk/app-debug.apk...           4.1s
Syncing files to device sdk gphone64 x86 64...                      90ms

Flutter run key commands.
r Hot reload. 🔥🔥🔥
R Hot restart.
h List all available interactive commands.
d Detach (terminate "flutter run" but leave application running).
c Clear the screen
q Quit (terminate the application on the device).

A Dart VM Service on sdk gphone64 x86 64 is available at: http://127.0.0.1:37057/fnUx2s7bfdU=/
The Flutter DevTools debugger and profiler on sdk gphone64 x86 64 is available at: http://127.0.0.1:9100?uri=http://127.0.0.1:37057/fnUx2s7bfdU=/

```

3. 動作確認<br>
Hostの[AndroidEmulator](https://developer.android.com/studio/run/managing-avds?hl=ja)でApplicationの動作確認を実施する。