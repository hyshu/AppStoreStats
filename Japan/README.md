統計の前提条件
----

## 調査対象
調査を試みた際に以下の条件を満たしていたiOSアプリが対象となった。

1. 日本の App Store にあり、アプリ名、説明文のどちらかにひらがな、カタカナを含む。説明文は日本語の文章が一文以上あることも条件
2. ゲーム以外のアプリ  
ゲームカテゴリーに公開されているアプリは全て対象外となる。  
ただしゲームカテゴリー以外で公開されており、知育やクイズなど、遊びが主題ではないものは調査対象に含む
3. 最新のiOSのiPhoneにインストールが可能
4. 無料アプリ
5. アプリが公開されている

アプリの一覧は[CatchApp](https://catchapp.net)の、「新着アプリから探す」の「総合」で「リリース日」順にし、「無料のみ」「日本語」を選択したものを使用している。  
アプリの公開年は [iTunes Search API](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/iTuneSearchAPI/index.html) により判断している。

ただし [iTunes Search API](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/iTuneSearchAPI/index.html) はごく稀に誤った公開年を返すことがある（年間十数本程度）。  
そのため、明らかに App Stote ID の数字が他よりも小さいアプリは [App Annie](https://www.appannie.com/jp/) などのアプリ調査サービスから公開年を判断している。

## 調査方法
1. iPhoneにアプリのダウンロードおよびインストールを行い、起動
2. [iTunes 12.6.5.3](https://support.apple.com/en-us/HT208079) で Windows PC にアプリをバックアップ
3. アプリのバックアップを解凍（拡張子は `.ipa` だが、実際はZIP圧縮ファイル）
4. ファイル名を元に判別

## フレームワークの判別
### パスによる判別
解凍したバックアップのパスにおいて、それぞれ以下の正規表現に合致する場合、そのフレームワークを用いて開発をしていると判断した。

#### Flutter
`^Payload/[^/]+.app/Frameworks/Flutter.framework/Flutter$`

#### React Native
`^Payload/[^/]+.app/[^/]+.jsbundle$`

#### Unity
`^Payload/[^/]+.app/Frameworks/UnityFramework.framework/UnityFramework$`

#### Xamarin
`^Payload/[^/]+.app/Xamarin.iOS.dll$`

### バイナリー解析による判別
バイナリー解析は、以下の手順で、バックアップから実行ファイルを解凍して行う。
1. バックアップのパスにおいて、以下の正規表現に合致する`Info.plist`ファイルを解凍  
  `^Payload/[^/]+/Info.plist$`
2. 解凍した`Info.plist`にある`CFBundleExecutable`キーに対応する実行ファイル（ここでは`$ExecutableFile`とする）を解凍
3. 解凍した実行ファイルに対し、以下のコマンドを実行  
  `oTool -L $ExecutableFile`

3の実行結果に特定の文字列が含まれるかによって、フレームワークを判別する

#### SwiftUI
`/System/Library/Frameworks/SwiftUI.framework/SwiftUI`

## WebView主体アプリの判定条件
WebView主体アプリとは、アプリの主要となる画面の多くにWebViewが設置されており、そこにWebページ（Webサイトからの読み込み、またはアプリ内に格納されたHTMLファイル）を表示しているアプリを指す。

主要となる画面とは、アプリの利用者が主に滞在するであろう画面を指す。  
（多くの場合、ログイン画面やアプリの説明画面があれば、その後の画面）

WebView主体であるかはアプリの主要となる画面の半分以上がWebViewかで判断している。  
1. タブUIが用いられているアプリは、タブ一つを一画面として判断
2. カルーセルUIが用いられているアプリは、スライドによって移動できる画面一つを一画面として判断
3. タブUIもカルーセルUIも使用しておらず、画面に数個のボタンが設置されているUIは、ボタンを押して遷移した後の画面を一画面として判断

法人契約が必要なアプリや、住所の入力が必須であるアプリ、その他会員登録が困難であるとみなしたアプリは、ログイン画面のみで判断する。  
（アプリの説明画面や「ログイン」ボタンからログイン画面に遷移する形式のものであっても、ログイン画面のみで判断）

画面内のボタンや画像、何もないところを長押しし、iPhoneが振動した場合にWebViewであると判断している。  
WebViewが一つの画面の一部にのみ使用されている場合は、スクロール分も含めておよそ半分以上がWebViewである時のみ、その画面はWebViewとして計算している。

これは手動による判定となっており、人為的な誤りも含まれる。  
判定漏れがあった場合は、ある程度まとまった段階で集計結果を更新する。

また、iPhoneにインストールしたアプリは定期的にアップデートを行なっている為、アップデートによってWebView主体アプリであるかが変化している場合、気付かずに集計する可能性がある。  
その為、WebView主体アプリに関しては正しい統計を作ることが出来ない。あくまでも目安として判断していただきたい。

## `apps.json`について

`apps.json`に実際に調査したアプリを羅列している。  
`id`キーが App Store ID、`classify`キーが`confirmed`のものが調査対象となったアプリとなっている。  
`ignore`のものは手動で調査対象外に分類したアプリで、本来含める必要は無いが、どういったアプリを調査から外したのかが分かりやすいので残している。

## 備考
本調査はリバースエンジニアリングを用いている。  
その為アプリを特定できるような情報は載せておらず、リバースエンジニアリングによって得られた情報は統計の作成用途でのみ用いている。

統計の全ての表において、比率の合計が丸め誤差で100%にならない場合も100%としている。