# Ansible Role: Nginx\_Install

# Trademarks
-----------
* Linuxは、Linus Torvalds氏の米国およびその他の国における登録商標または商標です。
* RedHat、RHEL、CentOSは、Red Hat, Inc.の米国およびその他の国における登録商標または商標です。
* Windows、PowerShellは、Microsoft Corporation の米国およびその他の国における登録商標または商標です。
* Ansibleは、Red Hat, Inc.の米国およびその他の国における登録商標または商標です。
* pythonは、Python Software Foundationの登録商標または商標です。
* Nginxは、Nginx Software Inc. の米国登録商標です。
* Crossplaneは、Upbound, Inc.の登録商標または商標です。
* NECは、日本電気株式会社の登録商標または商標です。
* その他、本ロールのコード、ファイルに記載されている会社名および製品名は、各社の登録商標または商標です。

## Description

本Ansble Roleは"Nginx"のインストールを行います。
対象バージョンは以下のバージョンです。

- RHEL 7

設定内容は以下のとおりです。  

- yumインストール前の準備  
    - yumを構成する
- packageインストール前の準備  
    - ターゲットマシンには、c++コンパイル環境、つまりgcc、gcc-c++がインストールされている必要があります。
- インストール実施  
    - Nginxのインストール  
    - 環境変数などの設定  

## Supports

- 管理マシン(Ansibleサーバ)
  - Linux系OS（RHEL 7 または 8）
  - Ansible バージョン2.8 または 2.9
  - Python バージョン2.7 または 3.6

- 管理対象マシン(インストール対象マシン)
  - RHEL 7

## Requirements
- 管理マシン(Ansibleサーバ)
  * 管理対象マシンとroot権限でSSH通信可能であること。

## Role Variables

Role の変数値について説明します。

### Mandatory Variables

以下の変数は、実行時に必ず指定します。

- 共通変数
  * ''VAR\_Nginx\_yumInstall'': nginxをインストール、yumインストールの場合はtrue、パッケージインストールの場合はfalse (bool, 例：true)

非Yumインストールの場合、以下の変数の設定が必要です。

  * ''VAR\_Nginx\_package\_dst'': インストールパッケージのコピー先 
    (string, 例："/home/nginxPkg")
  * ''VAR\_Nginx\_rpmList'': rpmパッケージリスト 
    (list, 例："- pcre-devel-8.32-17.el7.x86_64.rpm")  
  * ''VAR\_Nginx\_pkgList'': .tar.gzパッケージリスト  
    (list, 例："- zlib-1.2.11.tar.gz")  
  * ''VAR\_Nginx\_nginxPkg'': nginxインストールパッケージ名  
    (string, 例："nginx-1.16.1.tar.gz")  

### Optional variables

- Nginxインストールの関連変数
  * ''VAR\_Nginx\_package\_src'': 非yumインストールの場合、インストールパッケージのコピーソースを指定します。
    (string, "/home/nginxPkg") 
  * ''VAR\_Nginx\_nginxInstallPath'': 非yumインストールの場合のNginxインストールパス。インストールパスは「/ nginx」で終わる必要があります。パスにスペースを含めることはできません。スペースがあると、nginxのmakeでエラーが発生します。
    (string, "/home/nginx")
  * ''VAR\_Nginx\_nginxGroup'': 非yumインストールする場合は、ユーザーグループを指定します。
    (string, "nginx")
  * ''VAR\_Nginx\_nginxUser'': 非yumインストールの場合、ユーザー名を指定します。
    (string, "nginx")

## Dependencies

特にありません。

## Usage

1. 本Roleを用いたPlaybookを作成します。  
2. 必須変数を指定します。  
3. 任意変数を必要に応じて指定します。  
4. Playbookを実行します。  

## Example Playbook

インストールとすべての設定を行う場合は、提供した以下のRoleを"roles"ディレクトリに配置したうえで、
以下のようなPlaybookを作成してください。

- フォルダ構成
~~~
  - roles/
    ・ Nginx_Install/
    ・ Nginx_OSSetup/
    ・ Nginx_Setup/
  - hosts
  - Nginx.yml
  - conf.yml
~~~

- マスターPlaybook サンプル「Nginx.yml」
~~~
# Nginx.yml
- hosts: Nginx
  roles:
    - role: Nginx_Install
      VAR_Nginx_yumInstall: true
      tags:
        - Nginx_Install
~~~


## Running Playbook
- extra-vars を利用する場合の実行例
~~~sh
ansible-playbook Nginx.yml -i hosts --extra-vars="@conf.yml"
~~~

- host_vars を利用する場合の実行例  
 host_vars で指定したグループ名が server1 の場合
~~~sh
ansible-playbook Nginx.yml  -i hosts -l server1
~~~

- 本Roleのみを実行する場合は、 --tags "Nginx_Install" を付け加える
~~~sh
ansible-playbook Nginx.yml -i hosts --extra-vars="@conf.yml" --tags "Nginx_Install"
~~~

# Copyright
Copyright (c) 2020 NEC Corporation

# Author Information
NEC Corporation
