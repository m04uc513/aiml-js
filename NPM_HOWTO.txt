# aiml-js の npm/github の操作

2020/06/22
A.Fujita

将来 npm に publish する可能性があるので、
適切な手順に沿って npm/github の操作を行っている。
このテキストは操作概要のログである。


## ０．まず github のリポジトリを作成する

[参考情報](https://qiita.com/KosukeQiita/items/cf39d2922b77ac93f51d)

npm にはリポジトリのURLを登録する項目があるので 
github のリポジトリを先に作成する必要がある（のだろう）。
github の WebUI で新しいリポジトリを作成した後、
下記のコマンドを実行した。

```shell
$ git clone https://github.com/m04uc513/aiml-js.git
$ cd aiml-js/
$ touch 0README.txt
$ touch NPM_HOWTO.txt
$ git status
$ git add 0README.txt NPM_HOWTO.txt
$ git status
$ git commit -m "add local README"
$ git log
$ git push -u origin master
```

これで作業場所の github リポジトリへの登録はできた。


## １．次に npm パッケージとしての初期化を行う

[参考情報](https://qiita.com/star__hoshi/items/51c6cc719793abfb337d)

cli コマンドをサポートする npm パッケージとしての初期化を行う。

```shell
$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help init` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (aiml) aiml-js
version: (1.0.0) 0.0.1
description: AIML to js and json
{
entry point: (index.js)
test command:
git repository: (https://github.com/m04uc513/aiml-js.git)
keywords: AIML javascript JSON
author: A.Fujita
license: (ISC) MIT
About to write to /Users/fujita/xtr/TinyBots/aiml-js/package.json:

{
  "name": "aiml-js",
  "version": "0.0.1",
  "description": "AIML to js and json",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/m04uc513/aiml-js.git"
  },
  "keywords": [
    "AIML",
    "javascript",
    "JSON"
  ],
  "author": "A.Fujita",
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/m04uc513/aiml-js/issues"
  },
  "homepage": "https://github.com/m04uc513/aiml-js#readme"
}


Is this OK? (yes) yes
$ vi package.json
$ diff -u package.json

- "main": "index.js",
+ "bin": {
+   "aiml-js": "bin/aiml-js"
+ },

$ npm install
npm notice created a lockfile as package-lock.json. You should commit this file.
up to date in 0.356s
found 0 vulnerabilities

$ mkdir bin
$ cat > bin/aiml-js
#!/usr/bin/env node
'use strict'

console.log('Hello World');
$ chmod a+x bin/aiml-js
$ node bin/aiml-js
Hello World
$ npm link
up to date in 0.701s
found 0 vulnerabilities

/usr/local/bin/aiml-js -> /usr/local/lib/node_modules/aiml-js/bin/aiml-js
/usr/local/lib/node_modules/aiml-js -> /Users/fujita/xtr/TinyBots/aiml-js
$ aiml-js
Hello World
$
```

これで aiml-js コマンドはローカルで起動・実行できる。

```shell
$ cat > .gitignore
.DS_Store
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   NPM_HOWTO.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
	bin/
	package-lock.json
	package.json

no changes added to commit (use "git add" and/or "git commit -a")
$ git add .gitignore bin package-lock.json package.json
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   .gitignore
	new file:   bin/aiml-js
	new file:   package-lock.json
	new file:   package.json

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   NPM_HOWTO.txt

$ git commit -m "add aiml-js cli command"
[master 4c1f1e5] add aiml-js cli command
 4 files changed, 36 insertions(+)
 create mode 100644 .gitignore
 create mode 100755 bin/aiml-js
 create mode 100644 package-lock.json
 create mode 100644 package.json
$ git log
commit 4c1f1e587dc8390bbd792c1c878390f865c84096 (HEAD -> master)
Author: Akito Fujita <akito_fujita@mvg.biglobe.ne.jp>
Date:   Mon Jun 22 22:05:01 2020 +0900

    add aiml-js cli command

commit 3726eecf148740c0fb7c9a5ea6d1b867a9e4eb46 (origin/master, origin/HEAD)
Author: Akito Fujita <akito_fujita@mvg.biglobe.ne.jp>
Date:   Mon Jun 22 21:17:11 2020 +0900

    add local README

commit 262977b2042ec75b2ec2efcde5e1f3d611e202d4
Author: m04uc513 <akito_fujita@mvg.biglobe.ne.jp>
Date:   Mon Jun 22 20:56:33 2020 +0900

    Initial commit
$ git push -u origin master
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 8 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (7/7), 941 bytes | 941.00 KiB/s, done.
Total 7 (delta 0), reused 0 (delta 0)
To https://github.com/m04uc513/aiml-js.git
   3726eec..4c1f1e5  master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
$
```