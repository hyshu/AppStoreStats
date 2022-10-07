2021年の統計
----

## 調査対象
2021年に公開され、調査を試みた際に前提条件を満たしていたiOSアプリ1万3722本が調査対象となった。

## アプリのインストール作業を行なった期間
2021年1月8日〜2022年6月30日

## フレームワークごとの比率
| 名前 | 本数 | 比率 | 前年比 |
| --- | ---: | ---: | ---: |
| SwiftUI | 2062 | 14.8% | +9.3% |
| Flutter | 1803 | 13.0% | +5.3% |
| React Native | 859 | 6.2% | +1.3% |
| Unity | 566 | 4.1% | +1.4% |
| Xamarin | 168 | 1.2% | -0.1% |
| 未分類 | 8450 | 60.8% | -17.0% |
| 合計 | 1万3908 | 100% | |

表の「未分類」には、まだ判別方法が確立していないJavascriptを用いたフレームワーク、UIKitなどが含まれる。

また、上記のフレームワークが、一本のアプリに併用して使われている場合はそれぞれのフレームワークに計上している。
1. SwiftUIと React Native が128本
2. SwiftUIとFlutterが41本
3. Flutterと React Native が8本
4. FlutterとUnityが6本
5. React Native とUnityが1本

以上、計186本分が調査対象となったアプリ1万3722本に加算されている。

## WebView主体アプリとフレームワークごとの比率
Javascript系フレームワークの仕様が変わったのか、去年まではWebView主体アプリと判別していたUIのアプリの、画面の何も無いところを長押ししてもiPhoneが振動しない事例が多く見つかった。  
これでは統計としての意味が無いので、原因が分かり、統計としてまとめられそうであれば更新する。

## 二つのフレームワークの併用アプリについて
今年は併用アプリが多いので特筆する。

SwiftUIと React Native を併用したアプリが大きく増えているが、同一のデベロッパーアカウントのものが大半だった。  
SwiftUIとFlutterの併用アプリにはそういった傾向は見られない。[Stripe](https://stripe.com/)という決済サービスを使用しているアプリが多かったので、これが理由の一つの可能性がある。

Flutterと React Native の併用アプリは、全てを調べたわけではないが[FlutterBoost](https://github.com/alibaba/flutter_boost)というライブラリーを使用しているようだった。  
アプリも起動したところ、Flutterの挙動と React Native らしい挙動が混在していた。

FlutterとUnityの併用は2020年もあったが、こちらは画面ごとにFlutterとUnityが切り替わる（React Native とUnityのものも同様）。