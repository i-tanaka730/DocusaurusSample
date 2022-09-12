## 0. はじめに
勉強不足なもので、スマホアプリの開発は最近全然やっていませんでした:rolling_eyes:
私の自宅にはWindows1台、Mac1台あるのですが、性能的にも新しさ的にもWindowsの方が上ですので、開発は主にWindowsで行っております。
でも、iOSアプリも作りたいなーと思い(スマホはiPhoneなので)、最近流行っている**Flutter**を使ってみることに。
せっかくなのでアウトプットとして、環境構築手順やらCodemagicについて当記事にまとめました。

## 1. Flutterとは
- Googleによって開発された**クロスプラットフォーム**の開発技術。
- つまりは**React Native**や**Xamarin**のライバル。
- **Android Studio** or **XCode** or **VSCode**に**Flutter SDK**をぶち込んで開発する。
- **Dart言語**を使って実装する。

## 2. Codemagicとは
- Flutter専用の**CI / CD**サービス。
- ソースをPushすることで、自動でビルドやテストを行ってくれる。
- iOSのビルドは基本Macでしかできないが、なんとCodemagicが代わりにビルドしてくれるらしい。
- **GitHub** or **GitLab** or **Bitbucket**を連携して利用する。


## 3. 環境構築
今回はAndroid Studioを使って環境構築していきます。
※Android Studioの環境構築手順については割愛します。私はVer4.0.1を日本語化したものを利用しました。

### 3-1. Flutterのダウンロード

以下の公式サイトにアクセスし、最新VerのFlutterをダウンロード＋解凍します。

- [**Flutter SDK releases**](https://flutter.dev/docs/development/tools/sdk/releases?tab=windows)

![1.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/57ccd76b-726e-bab0-4b00-1d2d61806f74.png)

### 3-2. パスを通す
解凍したFlutterを任意のフォルダに配置し、flutter\binまでのパスを通します。

```
C:\Users\xxx\flutter\bin
```

### 3-3. 環境診断コマンドの実行
コマンドプロンプトで`flutter doctor`を実行することで、Flutterを作成するための環境が整っているかチェックをしてくれるようです。
ここで一度コマンドを実行してみましょう。

```
PS C:\Users\xxx> flutter doctor
Doctor summary (to see all details, run flutter doctor -v):

[√] Flutter (Channel stable, v1.17.5, on Microsoft Windows [Version 10.0.18363.959], locale ja-JP)

[!] Android toolchain - develop for Android devices (Android SDK version 28.0.3)
    X Android licenses not accepted.  To resolve this, run: flutter doctor --android-licenses
[!] Android Studio (version 3.2)
    X Flutter plugin not installed; this adds Flutter specific functionality.
    X Dart plugin not installed; this adds Dart specific functionality.
[!] VS Code, 64-bit edition (version 1.47.3)
    X Flutter extension not installed; install from
      https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter
[!] Connected device
    ! No devices available

! Doctor found issues in 4 categories.
```

### 3-4. issueの解消

**! (issue)**が出たようなので解消してきます。
※「! No devices available」については、実機 or エミュレータを起動していないため表示されているだけです。現状は無視でOKです。

#### 3-4-1. [!] Android toolchain...
<font color ="red">**X Android licenses not accepted.**</font>

Androidのライセンス確認ができないとのことなので、エラーにもある通り、以下のコマンドを実行すればOKです。

```
flutter doctor --android-licenses
```

#### 3-4-2. [!] Android Studio...
<font color ="red">**X Flutter plugin not installed; this adds Flutter specific functionality.
X Dart plugin not installed; this adds Dart specific functionality.**</font>

FlutterとDartのプラグインがないとのことなので、以下の手順でインストールすればOKです。
<font color ="blue">
**① Android Studioを起動。**
**② [構成] > [プラグイン]を選択。**
**③ Flutterをインストール。**
　**※FlutterをインストールすればDartも一緒に入ってきますので、一気に2つ「X」を解消できます:innocent:**
</font>

![1.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/9e27d87a-d3f1-df05-75e3-071acc109ccf.png)

#### 3-4-3. [!] VS Code...
 <font color ="red">**X Flutter extension not installed; install from xxx**</font>

VSCodeにFlutterがないとのことです。今回はAndroid Studioを使うので関係ないですが、「X」なしの方がかっこいいので一応解消しておきましょう。以下の手順でインストールすればOKです。
<font color ="blue">
**① VSCodeを起動し、[拡張機能]を選択。**
**② Flutterをインストール。**
</font>

![3.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/f1c79213-a73c-40aa-8dfa-c3f2f261716d.png)

### 3-5. 環境診断コマンドの再実行

ここでもう一度`flutter doctor`を実行してみましょう。

```
PS C:\Users\ikuya> flutter doctor
Doctor summary (to see all details, run flutter doctor -v):

[√] Flutter (Channel stable, v1.17.5, on Microsoft Windows [Version 10.0.18363.959], locale ja-JP)

[√] Android toolchain - develop for Android devices (Android SDK version 28.0.3)
[√] Android Studio (version 4.0)
[√] VS Code, 64-bit edition (version 1.47.3)
[!] Connected device
    ! No devices available

! Doctor found issues in 1 category.
```

**やった！解消されました:clap:これで環境はOKです:relaxed:**

## 4. プロジェクト作成
環境が整った所で、Android StudioにてFlutterプロジェクトを作成していきます。
**※作成したプロジェクトはGitHubに登録してください。(すみません、手順は割愛で・・・)**

①「新規 Flutter プロジェクトの開始」を選択します。
![1.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/7f7a7a0f-ba0b-51b0-e307-be3a0358b818.png)

② 「Flutterアプリケーション」を選択します。
![2.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/82dae935-9dcc-f602-7492-85bb60976fac.png)

③ プロジェクト名等を入力し、「次へ」を押下します。
![3.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/0d52687f-f9b1-9080-9d60-19cdb4b958ef.png)

④ パッケージ名を入力し、「完了」を押下します。
![4.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/17e1f9db-c0fb-af31-d36f-855163b92af2.png)

⑤ 完了！
![キャプチャ.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/91f774ed-fdd3-f200-bcec-a49506b1af37.png)

**これでFlutterの空プロジェクトを作成できました:blush:**

## 5. Codemagicの設定
続いてCodemagicの設定を行っていきます。

- [**Codemagic SignUp**](https://codemagic.io/signup)

上記にアクセスし、以下の手順で進めます。
(黒背景に黄色の蛍光ペンは非常に見づらかった・・・:disappointed_relieved:)

① 「Sine up」を押下します。
![1.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/27e0dda5-6158-60e8-2ff0-3706c83e1c4a.png)

② 「Join using GitHub」を押下します。
![2.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/be030ab0-f5f9-a450-2b60-36aebf9f330e.png)

③ GitHub上のFlutterプロジェクトの「Set up build」を押下します。
![3.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/4bdbf76a-2f9f-884c-e54e-935a0538b0b6.png)

④ 「Start your first build」を押下します。
![4.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/0a45e091-dd02-986b-e4e2-63f78a5ab494.png)

⑤ 「Start new build」を押下します。
![5.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/3b2dc65b-a583-97c4-6268-18b92ed94be1.png)

⑥ ビルド中の画面になります。完了するまで待ちましょう:ramen:
![6.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/018706ad-1089-24de-9498-853d13f38f99.png)

⑦ 完了！
![7.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/9042e1a0-4850-2adc-f19e-cf8fa5b63ae1.png)

**なんとこれだけでアプリケーションファイルが出来上がりました:astonished:すごい！！**

## 6. 動作確認
apkファイルをダウンロードしてエミュレータにインストールしてみた所、うまく動きました:blush:

![1.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247638/6f310a28-39db-33b1-a0d2-13a45a20bd9d.png)


## 7. おわりに
環境構築にしろCodemagicの設定にしろ、かなり簡単に行えましたね！
環境によっては`flutter doctor`で他にもissueが出るかもしれませんので、メッセージでググってみてください:innocent:
次回はDartに踏み込んでアプリを開発していきたいと思います。

- [**microCMSで作るシンプルなメディアアプリ**](https://microcms.io/blog/flutter-micrcocms-tutorial/)

↑な感じでmicroCMSとも組み合わせたいなー。
何よりAndroidの実機がほしい:sob::sob::sob:

## 8. 参考
今回は以下の記事を参考にさせて頂きました。ありがとうございます！

- [**WindowsでのFlutterアプリ開発環境構築**](https://note.com/hatchoutschool/n/n224ed7e3c146)
- [**Codemagicの設定手順＆ハマりそうなところをまとめてみた**](https://qiita.com/yukitaka13-1110/items/2b4fb8a25c1559f3e278)
