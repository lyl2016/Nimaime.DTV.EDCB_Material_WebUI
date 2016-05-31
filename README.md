EDCB Material WebUI
===================

**EDCBのWebUIをMaterial Design Liteでマテリアルデザインっぽくします**  
[xtne6f氏](https://github.com/xtne6f/EDCB)の[9bdd0a0](https://github.com/xtne6f/EDCB/commit/9bdd0a0f0c72a24eb680b1f890bf54c46bd2e939)以降で動作します  
またファイル再生にffmpeg.exe,readex.exe、ライブラリ機能にlfs.dllが必要にです  
ffmpeg.exe,readex.exeを別途ダウロードしてください  
readex.exeのダウンロードはEDCBの[releases](https://github.com/xtne6f/EDCB/releases)のEDCB-tools-bin.zipから

###使い方
EDCBのReadme_Mod.txtの*[Civetwebの組み込みについて](https://github.com/xtne6f/EDCB/blob/work-plus-s/Document/Readme_Mod.txt#L410-L475)*をよく読み  
README.md以外をEpgTimerSrv.exeと同じ場所に、ffmpeg.exeとreadex.exeをToolsフォルダに入れてください  
[HttpPublicFolder](https://github.com/xtne6f/EDCB/blob/work-plus-s/Document/Readme_Mod.txt#L433-L436)を設定している場合はHttpPublicの中身をそこに  
※HttpPublicFolderの任意のフォルダに入れる場合**apiフォルダ**だけは***HttpPublicFolder直下***に入れてください  

###テーマカラー
テーマカラーを変えることが出来ます  
[MDLのcustomize](http://www.getmdl.io/customize/index.html)で色を選択し
cssをダウンロードしmaterial.min.cssを置き換えるか  
Setting\HttpPublic.iniのSETのcssに下部に表示されてる<LINK>タグを追加して設定してください  
※一部(border周り)が置き換えただけでは対応できない部分があります(.mark)  
気になる方はcssをuser.cssに記述してください  
色は[Material design](http://www.google.com/design/spec/style/color.html#color-color-palette)から選択することをお勧めします  
.markのborderはA700を指定しています

###ライブラリ
録画保存フォルダのファイルを表示・再生します  
**LuaFileSystem(lfs.dll)が必要です**  
lfs.dllはxtne6f氏の[build_memo.txt](https://gist.github.com/xtne6f/f9b6f19c10cd146fe580)を参考にビルドしました  

必要に応じてSetting\HttpPublic.iniのSETに以下のキー[=デフォルト]を指定してください  
`batPath[=EDCBのbatフォルダ]`  
録画設定でこのフォルダの.batが選択可能になります  
\# 変更する場合必ずフルパスで設定

`LibraryPath=1`  
ライブラリに表示するフォルダの読み込み設定を録画保存フォルダ(Common.ini)からHttpPublic.iniに変更します  
\# Common.iniと同じ形式で指定してください  
\# 例

    [SET]
    RecFolderNum=2
    RecFolderPath0=C:\DTV
    RecFolderPath1=C:\hoge

`xprepare[=48128]`  
転送開始前に変換しておく量(bytes)

**※ffmpegとreadexのデフォルト値がToolsフォルダに変更になりました※**  
`ffmpeg[=Tools\ffmpeg]`  
ffmpeg.exeのパス

`readex[=Tools\readex]`  
readex.exeのパス  

`ffmpegoption[=-vcodec libvpx -b 896k -quality realtime -cpu-used 1 -vf yadif=0:-1:1 -s 512x288 -r 30000/1001 -acodec libvorbis -ab 128k -f webm -]`  
ffmpegのオプション  
\# -iは指定する必要ありません  
\# リアルタイム変換と画質が両立するようにビットレート-bと計算量-cpu-usedを調整する

次は**[MOVIE]**で指定  
`HD[=-vcodec libvpx -b 1800k -quality realtime -cpu-used 2 -vf yadif=0:-1:1  -s 960x540 -r 30000/1001 -acodec libvorbis -ab 128k -f webm -]`  
ffmpegのオプションのHD用設定  
\# HDと言いつつデフォルトでは960x540なのでバランスを見つつHDに調整してください

#####サムネ
HttpPublicFolderのthumbsフォルダにファイル名.jpgがあるとグリッド表示の時にサムネを表示します  
サムネの作成はToolsフォルダにバッチ例を同梱してます

#####ファイル再生
* トランスコードするファイル(ts)もシークっぽい動作を可能にしました(offset(転送開始位置(99分率))を指定して再読み込み)  
また総再生時間が得られないためシークバーは動かしてません  
* スマホではtsの場合再生終了のフラグが得られないのか自動再生が動きません  
* 番組・予約詳細ページ  
容量確保録画の場合シークっぽい動作はできません
* 録画結果ページ  
録画結果(GetRecFileInfo())からファイルパスを取得してます  
リネームや移動していると再生することが出来ません

#####番組表の隠しコマンド
`hour=整数`  
開始時間を指定

`interval=整数`  
表示間隔を指定

`chcount=整数`  
読み込むチャンネル数を一時的に変更  
\# showが有効時は非表示のチャンネルを含みます

`show=`  
非表示指定したチャンネルを読み込む(サイドバーで表示・非表示)  
\# 値は指定する必要はありません  

以上をgetメゾットで取得しますurlに含めてください  
chcountとshowは週間番組表では使えません  
スマホのブックマークなどでの使用を推薦(設定によっては軽くなるかも)

###注意
チャンネルが増えたりしたら設定を保存しなおしてください(番組表に表示されません)  
tkntrec氏版をお使いのかたは必ず設定のtkntrec氏版を**有効**にしてください  
有効せずに使用しEPG予約を変更しようとすると番組長などの追加機能の設定がデフォルトにされます

###動作確認

- PC
  - Chrome
  - ~~Opera~~
  - Vivaldi
  - firefox
- Android
  - Chrome
  - Opera

※IEでも基本的に動作すると思いますがおすすめしません  

###その他
このプログラムの使用し不利益が生じても一切の責任を負いません  
また改変・再配布などはご自由にどうぞ

####Framework & JavaScriptライブラリ

* [Material Design Lite](http://www.getmdl.io)
* [Material icons](https://design.google.com/icons/)
* [dialog-polyfill](https://github.com/GoogleChrome/dialog-polyfill)
* [jQuery](https://jquery.com)
* [jQuery UI](https://jqueryui.com)
* [jQuery UI Touch Punch](http://touchpunch.furf.com)
* [Hammer.JS](http://hammerjs.github.io)
* [jquery.hammer.js](https://github.com/hammerjs/jquery.hammer.js)
* [LuaFileSystem](https://keplerproject.github.io/luafilesystem/) ([ソース](https://github.com/keplerproject/luafilesystem/releases))
