ビッグデータを支える技術メモ

・ubuntuはLinux上で使えるOS
・Multipass launch -n primary　以降の記述がいまいち意味不明だし、書いても機能しない
・-y gcc python3-dev python3-venvは意味不明
・venvとは、Pythonの標準の言語処理系が持つ機能の一つで、システム上にPythonが動作する仮想的な環境（virtual environment）を作り出すもの。同じシステム上に複数の独立した環境を構成して使い分けることができる。
・まずpipとpip3の違いを理解していなかった。（ubuntuの仮想環境だとPython2とPython3が両方入っている。詳しくは以下↓
https://www.bioerrorlog.work/entry/install-pip-pip3-ubuntu#pip%E3%81%A8pip3%E3%81%AF%E9%81%95%E3%81%86

・Is Home
→Is: command not found

・jupyter lab --ip 0.0.0.0 --no-browser --noteboook-dir=notebooks入力。
→エラーCommand 'jupyter' not found, but can be installed with:
sudo snap install jupyter       # version 1.0.0, or
sudo apt  install jupyter-core  # version 4.6.3-3
See 'snap info jupyter' for additional versions.

・再度jupyter lab --ip 0.0.0.0 --no-browser --noteboook-dir=notebooks入力
Error executing Jupyter command 'lab': [Errno 2] No such file or directory
→pip3 install jupyterlab→解決せず→https://github.com/jupyterlab/jupyterlab/issues/3921を参照
→解決せず→https://na1.tech/258/を参照→解決。sudo apt -y upgradeをしてなかったから？

サーバーは立ち上がるようになったものの、クロムやsafari上でアクセス拒否
→jupyter lab --generate-configで設定ファイルを見てみる→特に怪しいところなし→どうもできないのでmultipass stop primary、multipass delete primary 、multipass purgeでインスタンス自体を一度完全に削除→第7章最初からやり直し→同じところでエラーサーバーアクセス拒否→ primaryをlaunchするときにメモリとディスク容量のサイズを拡大して指定することで解決

・sparkではcoalesce()関数を使うと、分散処理の結果を1箇所に集めて1つのファイルにまとめることができる
・#デスクトップにコピー　!cp ./export/*.csv ~/Home/Desktop
・chmod でファイルの管理者情報を変更することができる。
・ssh username@hostnameでsshコマンドを使ってログインすることができる。ubuntuサーバーを立てる場合は、usernameは自動でubuntuとなり、ホストネームは自動的にAWSコンソールのPublic DNSに記載されている。
・sftp はファイルを送るためのコマンド
・sudo gpasswd -a <username> <group_name（今回の場合は”docker”）>でsudo権限じゃなくてもdockerコマンドがpermission deniedではなくなる。
・dockerファイルをtarファイルにするときは、docker save <image名> > <tarファイル名>というコマンドですることができる。
・tarファイルをdockerに戻すときは、docker load < <tarファイル名>で戻すことができる
・Linuxでは、Dockerは/var/lib/dockerに保存されている。

・AWSのマネジメントコンソールからEBSの拡張をするだけだと、no space lett on device というエラーが出る。
→ボリューム全体のサイズは拡張されていますが、ルートのパーティションのサイズは変わってません。よって、sudo growpart /dev/xvda1の拡張を行う。それでも治らず。→追加でファイルシステムの拡張を行う（参考文献→https://tech-note-meeting.com/2021/04/28/post-888/）→解決！
・ubuntuでユーザーを作りたいときはsudo add userで追加できる
192.168.64.2 


プロキシサーバーとは使用すると、ブラウザで直接Webサイトにアクセスせずに、プロキシサーバーにアクセス、プロキシサーバーが代わりに目的のサイトにアクセスしてデータを受け取り、ブラウザにデータを渡して表示させます。
