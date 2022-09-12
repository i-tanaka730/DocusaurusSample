## 0. はじめに

ANTLRを使って少し構文解析を行いましたので、備忘録として記事にしてみました。

## 1. ANTLRとは
**アントラー**と読みます。
構文解析を行うためのツールになります。
解析のためのルールを記述する**grammar(グラマー)**をいうファイルを作成することで、与えられたテキストをそのルールに応じて振り分けすることができます。
グラマーは**lexer(レクサー)**と**parser(パーサー)**という文法を使って記述します。
以下は[**公式サイト**](https://www.antlr.org/)のサンプルgrammarファイルになります。

```:sample.g4
grammar Expr;		

/**
 * parser
 */
prog:	(expr NEWLINE)* ;
expr:	expr ('*'|'/') expr
    |	expr ('+'|'-') expr
    |	INT
    |	'(' expr ')'
    ;

/**
 * lexer
 */
NEWLINE : [\r\n]+ ;
INT     : [0-9]+ ;
```

#### 1-1. lexer
- lexerとは、**字句解析器**になります。
- 正規表現を使って「この変数にはこんな値が入るよ」を定義します。
- 上記サンプルgrammarファイルの「lexer」部分になります。

#### 1-2. parser
- parserとは、**構文解析器**になります。
- 「解析する構文はこんな感じで振り分けるよ」を定義します。
- 上記サンプルgrammarファイルの「parser」部分になります。

## 2. 環境構築

※VSCodeはインストールされている前提とさせて頂きます。
VSCodeに、**神**拡張機能である[**ANTLR4 grammar syntax support**](https://marketplace.visualstudio.com/items?itemName=mike-lischke.vscode-antlr4)をぶち込みます。

元々環境構築やデバッグが少し手間だったのですが、これを使えばめちゃくちゃ楽になります。
自分が使った機能としては、以下のようなものがあります。

- シンタックスハイライト機能
- デバッグ機能(構文解析結果がイメージで確認できる)
- 言語に応じたジェネレート機能

他にもたくさんのことができるようです。詳しくは[**vscode-antlr4**](https://github.com/mike-lischke/vscode-antlr4)をご参照ください。

![キャプチャ.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/d76631a3-81fc-e392-191c-02ed8d94b045.png)


## 3. グラマーファイルを作成する
それでは、グラマーファイルを作成していきます。
lexerとparser、1ファイルで書くことも、別々のファイルで書くこともできますが、当記事では1ファイルで書いています。
そして今回は子供達にも喜んでもらえるよう、きめつちっくな構文を解析してみます。


| 入力文字列(例) | | 種別 | 番号 | 名前 | オプション |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| 水の呼吸拾壱の型凪 |  | 水  | 拾壱  | 凪  | -  | 
| 雷の呼吸壱の型霹靂一閃(六連)  |  | 雷 | 壱  | 霹靂一閃  | 六連  | 

「入力文字列」を与えることで、「種別」「番号」「名前」「オプション」として解析できるようにします。
オプションてなんやねんて感じですが、六連なのか八連なのか~~神速なのか~~、名前だけでは情報が不足する場合もあるので。。。
結果、グラマーファイルは以下のように書いてみました。

```:Breathing.g4
grammar Breathing;

/**
 * parser
 */
breathing :
    // [TYPE]の呼吸[NUM]の型[NAME]([OPTION])
    type '\u306e\u547c\u5438' num '\u306e\u578b' name ('('option')')?
    ;

type :
    TYPE
    ;

name :
    (ANY)+
    ;

num :
    NUMBER
    ;

option :
    (NUMBER | ANY)+
    ;

/**
 * lexer
 */
TYPE :
    // 水炎雷風岩蟲花恋蛇音獣月日
    [\u6c34\u708e\u96f7\u98a8\u5ca9\u87f2\u82b1\u604b\u86c7\u97f3\u7363\u6708\u65e5]
    ;

NUMBER :
    // 壱弐参四五六七八九拾
    [\u58f1\u5f10\u53c2\u56db\u4e94\u516d\u4e03\u516b\u4e5d\u62fe]+
    ;

ANY : 
    .
    ;
```

## 4. テキストを与えてデバッグしてみる

#### 4-1. launch.jsonを作成

`.vscode`フォルダ配下に、`launch.json`ファイルを作成し、以下のように記載します。

```launch.json
{
  "version": "1.0.0",
  "configurations": [
      {
          "name": "Debug ANTLR4 grammar", // 任意の名前
          "type": "antlr-debug",          // 決め打ち(多分)
          "request": "launch",            // 決め打ち(多分)
          "input": "input.txt",           // 入力となるテキストを記述したファイル
          "grammar": "Breathing.g4",      // 使用するグラマーファイル
          "printParseTree": true,         // デバッグ結果をデバッグコンソールに表示するか
          "visualParseTree": true         // デバッグ結果をグラフィカルなツリーで表示するか
      },
    ]
}
```

#### 4-2. 凪なデバッグ実行

`input.txt`に、以下を入力して、VSCodeでデバッグ実行します。

```input.txt
水の呼吸拾壱の型凪
```

以下のような実行結果になればOK！

![2.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/b1760aa5-abeb-ad06-bd91-06eb40d716c8.png)

#### 4-3. 霹靂一閃(六連)なデバッグ実行

`input.txt`に、以下を入力して、VSCodeでデバッグ実行します。

```input.txt
雷の呼吸壱の型霹靂一閃(六連)
```

以下のような実行結果になればOK！

![3.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/31ee2507-dec7-d646-7ccc-9fd8f3f40a9c.png)

## 5. プログラミング言語に合わせたコードを出力する

#### 5-1. settings.jsonを作成

`.vscode`フォルダ配下に、`settings.json`ファイルを作成し、以下のように記載します。
なお、他にも色々な設定することができます。詳しくは[**vscode-antlr4(extension-settings)**](https://github.com/mike-lischke/vscode-antlr4/blob/master/doc/extension-settings.md)をご参照ください。

```settings.json
{
	"antlr4.generation":{
		"mode": "external",   // 決め打ち(多分)
		"language": "CSharp", // 出力したいプログラミング言語名
		"listeners": true,    // listener形式で出力するか
		"visitors": false,    // visitor形式で出力するか
		"alternativeJar": "antlr-4.9-complete.jar", // ANTLRのjarファイル(※)
		"outputDir": "parser" // 出力先のフォルダ名
	}
}
```

(※)ANTLRのバージョンを指定したい場合に設定します。指定しないと4.8で出力されます。ANTLRの各バージョンのjarは[**こちらから**](https://www.antlr.org/download/)ダウンロードできます。

#### 5-2. コードを出力する

グラマーファイルを保存(Ctrl + s)すれば、`outputDir`で指定したフォルダにコードが生成されます。

![1.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/7873b5dc-0fd2-c9a2-4888-9460c256abc0.png)


## 6. GitHub
[**antlr_breathing**](https://github.com/i-tanaka730/antlr_breathing)

## 7. おわりに

個人的に普段は使わないような技術なので、新鮮な気持ちで取り組むことができました:innocent:
当技術をピンポイントで使うことはあまりないかもしれませんが、何かしらに応用できればと思います:innocent:

## 8. 参考
https://github.com/antlr/antlr4/blob/master/doc/index.md

https://future-architect.github.io/articles/20200903/

https://www.gitmemory.com/issue/mike-lischke/vscode-antlr4/107/605636407
