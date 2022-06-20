# 米国AI開発者がゼロから教えるDocker講座memo
・$は環境変数であることを表す指標のようなもの
・~はホームディレクトリを示している
・touchは新しいファイルを作成、mkdirは新しいフォルダの作成
・pwdは現在のディレクトリを表示
・cd ..で一つ前のフォルダに戻る
・rmでファイル削除　rm -rでフォルダ削除
・docker pull <image名>でdocker imageをdocker hubから取ってくる
・docker run <image>名でdocker imageから一つのdockerを作る。hostに指定したdocker imageがない場合は自動でdockerhubに探しに行く
・docker psで作られているアクティブなコンテナの一覧を見ることができる。-aだとコンテナ全て
・docker run -it ubuntu bashでubuntuのドッカーを実行できる（めっちゃよく使う）-itはI=input、つまりdockerというコンテナへホストからの入力チャネルを開く役割 t=表示が綺麗になる
・コンテナに入る docker exec -it <Docker名> bash
・exitだとdockerで動かしているプログラムを切ってログアウトするが、ctl + p + qはプログラムをそのままの状態にしてログアウトする。exitした後はrestartしないとログインできない。ctl + p + qの場合はattach <docker名>で再度ログインすることができる。
・terminalの履歴消去＝reset と打つとできる
・docker tag<source(Image) name> <new name>でイメージ名を変更できる
・docker push <image>でdockerhubにイメージをプッシュすることができる。
・docker rmi でイメージを削除できる
・止まっているコンテナの全削除=docker system prune
・一回作ってすぐ捨てる場合は、docker run —rm <image名>にすると良い
・cat ファイルの内容を参照
・echoファイルに特定の内容を書き込む
・「.」はcurrent directoryのことを示している。
・ベストプラクティス＝DockerファイルのRUN数（レイヤー数）はできるだけ少なくする（DockerFileではRun、Copy、Addの分だけ新たにイメージレイヤーが作られる）
・aptはubuntuの命令の一つ。apt update=パッケージの最新リストをダウンロード、apt install <パッケージ名>でインストールする。
・Dockerの環境を作っている最中は細かいアクションごとにRUNをすることで以前ダウンロードしたものをキャッシュしてそのまま利用できる。一方で、最後の完成品としてのDockerfile自体には後でできるだけレイヤー数が少なくなるようまとめ直す必要がある。
・CMDは原則最後に記述。CMDはdockerのデフォルトのコマンドを指定する。
・docker build .　でビルドするのが普通。
・du -sh <ファイル名>でファイルサイズを参照できる
・dockerファイルが入ったフォルダには余計なフォルダを入れない
・ADDとCOPYはほぼ一緒の機能だが、tarの圧縮ファイルを使うときのみADDを使う。（すごく大きなファイルを添付してdaemonに送ってDocker Imageにしないといけない時に、一旦tarで圧縮して送る必要がある。その時にADDで記載することで、のちに自動で解凍してくれる。）
・mvはファイルの名前を変更するコマンド
・docker build -f <指定すべきdockerfileのpath> <ディレクトリ名>でフォルダ外にあるdockefileを参照してbuildすることができる。
・ENTRY POINTはDocker構成時に上書きできないverのCMD。ENTRYPOINTがある時は、ENTORYPOINTで記載してない部分のみCMDで記載する
・docker run -v <マウント元のpath>:<マウント先のpath> <image名>でコンテナ内からマウントされたホストのフォルダにアクセスできるが、これはコピーされているわけではなく、マウント先のファイルを弄るとhostの同ファイルの内容も変更されてしまうので注意。
・Command + tで新しいターミナルのタブを開くことができる。
・Docker run -u <used id>:<gorup id>でユーザーを指定してログインすることができる。（なお、<user id>に直接番号を入力しなくても、$(id -u)や(id -g)を入力することで、自動的にLinux OSの方でユーザーIDとグループIDを補完してくれる）
・-u $(id -u):(id -g)の形で覚えるべき
・-p＜ホストのポート＞：＜コンテナのポート＞を記載することで、アプリケーションがコンテナ内で動いているときに、ホストとそのコンテナをつなげることができる（publishすることができる）
・
・同じコンピュータ内で動作する複数のソフトウェアのどれが通信するかを指定するためにポート番号（port number）が用いられ、これを略してポートということがある。

・パッケージをコンテナ内にインストールするときは他の人がアクセスしやすいようにroot直下ではなく、/optの下に置くことが多い
・Dockerfileを書くときは、最初に何を書くべきかが全部はわかっていないことが多い。よって、実際にコンテナに入って作業を繰り返しながらうまくいったらHostに戻ってDockerFileに記載するという流れをとることが多い。
・Dockerfileでファイルを削除するときは、-fをつけることでインタラクティブなやりとりを避けることができる。
・sh <installerのパス>というコマンドを打つ頃で特定のパッケージのインストールを実行できる（今回はanacondaのインストールに用いた）
・shを使うときはとりあえず sh -x <installerのパス>を実行することで、shのオプションでどのようなものがあるのかを見ることができる。その中でも重要なのは-pと-b。-pはパッケージのインストール先を指定することができる。-bはインタラクティブなやりとり（インストール途中にyesと答えるなど）を回避することができる。
・PATHを通すとは（<command> not foundの原因）＝環境変数$PATHにパスを追加することで、コンピュータがプログラムをそのパスから自動的に探してきてくれるようになる。exportコマンドでパスを通す。export PATH=<path/to/something>:$PATHというコマンドでパスを追加できる。
・環境変数$PATHの中にはターミナルで何かコマンドが打たれた時に、そのコマンドのプログラムを探しに行くパスが入っている。
・パスの一覧を見るにはecho $PATHとうつ。通すべきパスのディレクトリは大抵binというディレクトリの下にあることが多い。
・echoは画面に文字列や数値、変数を表示するLinuxコマンドだ。 「エコー」と読み、そのまま繰り返す「こだま・反響」を意味するコマンドだ。
・dokcer composeとはdocker runコマンドが長くなる時に使う。
・docker　composeを使うときは、ファイル指定は相対パスで書くこと
・dockerを使うときは一つのサービスにつき、一つのコンテナを使う。

エラー集
・Docker buildでpip not found のエラー
→ENV でパスを通す際にanaconda3がanconda3になっており、nが抜けていた
・"LabApp.token=“]でno such file or directry
→"LabApp.token=“]ではなく、"LabApp.token=‘’“]だった。


