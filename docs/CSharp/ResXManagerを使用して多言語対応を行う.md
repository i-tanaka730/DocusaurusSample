## 0. はじめに
Visual Studioで多言語対応が必要なアプリケーションを開発する際、**ResXManager**という拡張機能が便利そうだったので、使用方法などを簡単にまとめてみました。
**ResXManager**は、各言語リソースファイルを開かなくとも、専用の画面で言語キー(文字列)を参照/追加/更新/削除できる機能になります。

## 1. ResXManagerをダウンロードする
Visual Studioメニューの「拡張機能 > 拡張機能の管理」から、**ResXManager**をダウンロードします。
※ダウンロード後、変更を反映するため、Visual Studioの再起動が必要かと思います。
![1.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/bac3814e-7662-ac36-c0ab-41d5d61a4c97.png)

## 2. 言語リソースファイルを追加する
まずは準備として、ソリューションエクスプローラーコンテキストメニューの「追加 > 新しい項目」からリソースファイルを作成します。
名前は何でもいいですが、私は「Strings.resx」としました。
![3.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/3051352a-eb57-3490-d03a-42f640d0cf9f.png)

## 3. ResXManagerを使ってみる
Visual Studioメニューの「ツール > ResX Manager」を選択すると、ResX Resource Managerが起動します。

### 3-1. デフォルトの言語を変更する
Configurtionタブの「Netural Resouces Language」から、デフォルトの言語を変更できます。
元々は英語になっているかと思いますので、日本語に変更しました。
![4.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/95b1c1c6-9e89-8db8-aebc-9849dfecd995.png)

### 3-2. 言語を追加する
Mainタブの「Add new language」から、言語を追加できます。
今回は英語・フランス語を追加してみました。
![5.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/bf2215f4-ea7a-7547-d83e-504ad9f86822.png)

対応するリソースファイル(Strings.en.resx、Strings.fr.resx)が自動で生成されます！
![6.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/709d93ac-0912-435d-cf83-5fee063dad27.png)

### 3-3. キー(文字列)を追加する
「Add new key」から、キー(文字列)を追加できます。
![7.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/f3a0fa37-373e-a73c-08ce-f8fa360204d5.png)

文字列を追加しない場合は、赤塗りで表示されます。
![8.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/cf95075e-67fe-e5c5-02ee-cb34f1892f46.png)

### 3-4. キー(文字列)を削除する
「Delete selected items」から、キー(文字列)を削除できます。
![9.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/53b1936a-04c5-5234-081d-b6641eac9cf8.png)

## 4. 出力してみる
```C#
class Program
{
	static void Main(string[] args)
	{
		// 日本語
		Console.WriteLine(Strings.HelloWorld);

		// 英語
		Thread.CurrentThread.CurrentUICulture = new CultureInfo("en-US");
		Console.WriteLine(Strings.HelloWorld);

		// フランス語
		Thread.CurrentThread.CurrentUICulture = new CultureInfo("fr-FR");
		Console.WriteLine(Strings.HelloWorld);
	}
}
```

![w.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/b7583614-9f05-b36c-6f70-cac048f0bbac.png)

## 5. おわりに
- 「〇〇言語に〇〇キーの文字列が設定されていない！」ということにすぐ気づけるのでとても便利ですね:innocent:(今回この拡張機能を使おうと思ったきっかけがコレでした)
- 私が必要としていた機能は上記に記載した範囲でしたが、他にも「キー(文字列)のコピー/ペースト」「エクセルへのインポート/エクスポート」 といったことが行えるみたいです:innocent:
