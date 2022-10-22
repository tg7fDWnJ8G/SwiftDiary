# Swift日記
---
[【2022/08/21】](#2022/08/21),[【2022/08/22】](#2022/08/22),[【2022/08/26】](#2022/08/26)
---
## <a id="#2022/08/21">【2022/08/21】</a>
昨日から何か新しいことをやろうと思い立って、また付属のSwift Playgroundsをいじり始めた。今まで、一通りのプレイグラウンドは触って、子どもたちにもやらせたりしていたけれど、Swiftのバージョンが5.5になり、以前のプライグラウンドから変わっていた。

Appギャラリーの「予定表」を色々眺めて見ることにした。@Published、ObservableObjectなど、更新時のレンダリングを自動でやってくれそうな仕組みの片鱗が見えて、よく出来ている感じ。ただ、文法がよくわからない。これでは、せっかくのPlaygroundsのサンプルを活かせない。どうしたものか。

ClassとStructの使い分けもよくわからない。機能の違いを一生懸命説明しているサイトはあるが、根源的にSwiftの設計者がどのように意図して2つを用意したのか。意図に沿うとどういう使い分けになるのか。とりあえず、以前から開いていたSwift.orgの言語ガイドに目を通すことにした。  
[https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html](https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html)

基本(Basics)の1/3がオプショナルに割かれている。型推論とかに絡んでいるのか、大ごとになっている感じ。要するに定義した変数に値が入っていない場合があるなら、折り込もうってことね。オプショナルでwrapするってのがあるか無いか不明な状態で、値が入っているならunwrapして使いなさい、ってのはその通りだけど、なかなかにやってみないと使いこなせるかわからない。

日付も変わるのし、そろそろ疲れたので、今日はこれでおしまい。

## <a id="2022/08/22">【2022/08/22】</a>
Swift言語ガイドの続き。

オペレータはそんな新しいものないか、と思っていたら、意外に面白かった。オプショナル関連のnilチェックとか、範囲のオペレータとかは興味深い。  
[https://docs.swift.org/swift-book/LanguageGuide/BasicOperators.html](https://docs.swift.org/swift-book/LanguageGuide/BasicOperators.html)

コレクションは、サッと見る感じにしたけど、Javaでコレクションフレームワークが一般的になって、基本的なところは言語仕様に近いところに取り込まれるようになってきたか。 Dictionaryのリテラルは面白い。  
[https://docs.swift.org/swift-book/LanguageGuide/CollectionTypes.html](https://docs.swift.org/swift-book/LanguageGuide/CollectionTypes.html)

コントロールフローは制御文。コレクションの存在が前提になっってきて、foreach的な処理が前面。switch文が、オプショナルと範囲のオペレータを組み合わさって、色々できる。まぁ、自分の好みの書き方に落ち着くんだろうけど。  
[https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html)

関数。returnの省略とか、引数のラベルはよくわからないし、さらに省略もあるともう。タプルを使った複数値のリターンはいいとして、タプルにオプショナルが組み合わさると訳がわからない。可変長もあるし。インアウト引数とかタプルも、微妙にオブジェクトの参照渡しを隠蔽している感じ。関数タイプが登場して、クロージャも出てくるよね、と。  
[https://docs.swift.org/swift-book/LanguageGuide/Functions.html](https://docs.swift.org/swift-book/LanguageGuide/Functions.html)

クロージャは、使いこなせばすごく便利なんだろうけど、そのために、型推論できるところは省略しまくれるようになっている。あとで読んでわかるんだろうか。  
[https://docs.swift.org/swift-book/LanguageGuide/Closures.html](https://docs.swift.org/swift-book/LanguageGuide/Closures.html)

ちょっと疲れて、とりあえず文字の時計アプリのサンプルが無いか探してみた。Xcodeや古そうな実装のものが多いが、参考にしたいと思えるものを見つけた。コンバインを使う、というのでどういうことなんだろう、と思ったら、期待通りの実装。  

SwiftUIで超簡単に時計アプリを作る  
[https://note.com/taatn0te/n/n74bd932b0704](https://note.com/taatn0te/n/n74bd932b0704)

Javaのコンカレントフレームワークは知らないけど、古い実装だと、runnable インタフェースを実装して、スレッドの中で1000ミリ秒休んで文字列を書き換えて、みたいになるけど、コストがかかると考えていた。

文字列を書き換えるコールバック関数を共通的なスケジューラに登録して、1秒ごととか間隔を設定して呼び出してもらう、みたいな処理がいいよな、と考えていたら、本当にそういう実装だった。スケジューラで処理を束ねるのでコンバインか。

Publish & Sbscribeのデザインパターンを知っていると、そうだよね、という感じではあるけど。非同期処理のフレームワークとしては、jQueryのDeferredが鮮烈だった。あまり使いこなせななったけど。

しかし、onReceiveやTimerのドキュメントがAppleの開発者サイトでなかなか行き着けない。Googleで検索すれば出てくるので、結果からはどのフレームワークのどのAPIコレクションにあるかはわかる。なかなかしんどい。onReceiveはViewで宣言されているようだが、TextはViewを継承しているのか、実装しているのか、Java Docのように追えないのでわからない。

## <a id="2022/08/26">【2022/08/26】</a>
時計アプリの続き。

ボタンを追加することにした。ボタンを押した時の動作は特に定義せず、アイコンとテキストを貼った。
Button {} label: {}という記載だけでも、色々省略されてこの表記になっているようだが、何がどう省略されているのかも理解できない。  
[https://developer.apple.com/documentation/swiftui/button](https://developer.apple.com/documentation/swiftui/button)

次に、過去にHTML5とBootstrapで作ったアプリを思い出し、3値から1つを選択するインタフェースを作ることにした。

HTMLだとグループ化したラジオボタンになるので、「ラジオボタン」や「ボタングループ」というキーワードでGoogleで調べたがなかなか引っかからない。Xcodeでの実装の仕方はあっても、swiftの実装が出てこない。

Swift Playgroundsのサンプルの「予定表」でViewを生成する際に、enumでフィールド名を定義して、ForEachで回す、ということをしていた。とりあえず、ボタン名のenumを定義した。しかし、Javaでもenumはあったはずだが、C系言語と合わせても使ったことがない。swiftだとこう書くのか、という感じ。  
[https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html](https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html)

さらに、ForEachがわからない。swiftの言語ガイドの制御文にはfor inしかない。普通に考えればコレクションや配列のメソッドで、処理を記載すれば、要素の数だけ繰り返して実行されるようなイメージだ。だけどサンプルをよくよく見ると、enumは引数になっている。いくらswiftでもそういう記法はあるまい、と思って見ていたら、SwiftUIのViewの一種で、コレクション等から複数のViewを作成して保持するコレクションコンテナというものだった。まさかViewの名前がForEachだったとはね。  
[https://developer.apple.com/documentation/swiftui/foreach](https://developer.apple.com/documentation/swiftui/foreach)

しかしラジオボタンの作り方が出てこない。そう言えば、通常、iOSだとどんな外観になっているか。3値からの選択であれば、ボタンが3つ並んで、そのうち1つだけが押せるようなインタフェースだ。そもそもラジオボタンというキーワードが違うのか。チェックボックスもiOSなら外観はトグルだし。

SwiftUIのAPIドキュメントを見ていたら、Pickerが目に留まった。複数の選択肢から選ぶんだからPickerってはわからなくもないけれど、値のピッカーと色のピッカーと日付のピッカーでは、UI実装の規模がかなり異なる。これが並んでいる、というのは粒度の点でかなり違和感がある。

とは言えこれだ。enumから作成する方法が丁寧に書かれている。文法もライブラリもよくわかっていないので、「$」付きの変数が出てきたり、CaseIterableやらIdentifiableやらキーワードが出てきたりすると、いちいち調べないとわからない。  
[https://developer.apple.com/documentation/swiftui/picker](https://developer.apple.com/documentation/swiftui/picker)

Var id: Self {self}の「Self {self}」はどういう意味なんだろう。


---
Copyright 2022   Takashi KOBAYASHI   All Rights Reserved.
