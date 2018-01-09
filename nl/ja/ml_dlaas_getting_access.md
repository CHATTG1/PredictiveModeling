---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 機械学習環境のセットアップと資格情報の取得

IBM Watson Machine Learning を使用するには、適切な機会学習環境を作成することと、その環境に固有の資格情報を取得することができなければなりません。

## 環境のセットアップ

IBM Watson Machine Learning を使用するには、IBM Cloud ダッシュボードで以下の項目のインスタンスをセットアップする必要があります。

- Watson Machine Learning
- Object Storage
- Apache Spark

これらの必須インスタンスに加えて、一部のノートブックおよびチュートリアルでは以下のインスタンスも使用されます。チュートリアルを使用するには、これらのサービスもセットアップする必要があります。

- Python Flask
- Natural Language Understanding
- 深層学習試験

### IBM Cloud インスタンスの作成

次のビデオを視聴して、IBM Cloud インスタンスの作成方法および資格情報の表示方法を確認してください。次に、ビデオの下にある手順に従って、独自の環境をセットアップしてください。資格情報を表示する手順については、『<a href="#retrieving-your-credentials">資格情報の取得</a>』セクションを参照してください。

<figure class="fignone" id="concept_bvb_fts_1cb__machinelearningsetup"><figcaption>図 1. <span class="ph"><a href="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" rel="external" target="_blank" title="このページに組み込まれているビデオにアクセスできない場合、YouTube Web サイトからビデオにアクセスできます。(新しいタブまたはウィンドウで開きます)">    <img src="images/video.png" alt="ビデオ・アイコン"></a>IBM Watson Machine Learning: Get Started</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/fm8gqguFD9g?rel=0" width="560">
<span>このビデオでは、機械学習環境のセットアップ方法の概要が説明されています。</span>
<param name="movie" value="https://www.youtube.com/embed/fm8gqguFD9g?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

Watson Data Platform インスタンスを作成するには、以下の手順を実行する必要があります。

1. [IBM Cloud にログインします](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter)。
2. ナビゲーション・パネルで**「データおよび分析」**をクリックします。

   データ・サービス中心の画面が表示されます。定期的にここに戻って、簡単に使用できる 1 ページからデータおよび分析サービスを使用して作業することができます。

3. **「分析」**タブをクリックし、**「インスタンス」**をクリックします。
4. **「新規インスタンス (New Instance)」**をクリックします。
5. インスタンスおよびデータ・ストレージを構成します。

   インスタンスの記述名を入力し、スペースを選択し、データ・プランを選択します (プラン比較および料金の詳細はこのページにあります)。IBM Cloud は、インスタンス名を使用してオブジェクト・ストレージ名を自動的に設定します。Spark プロジェクト用にデータ・ストレージがおそらく必要になるため、スペースとプランもここで選択します。

6. **「インスタンスの作成 (Create Instance)」**をクリックします。

インスタンスが開きます。

### Data Science Experience のプロジェクトへの既存 Bluemix インスタンスの追加

次のビデオを視聴して、Watson Machine Learning を使用するためにプロジェクトを作成してセットアップする方法を確認してください。

<figure class="fignone" id="concept_bvb_fts_1cb__machinelprojectcreate"><figcaption>図 2. <span class="ph"><a href="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" rel="external" target="_blank" title="このページに組み込まれているビデオにアクセスできない場合、YouTube Web サイトからビデオにアクセスできます。(新しいタブまたはウィンドウで開きます)">    <img src="images/video.png" alt="ビデオ・アイコン"></a>IBM Watson Machine Learning: Create a project for Watson Machine Learning</span></figcaption>

<object height="315" data="https://www.youtube.com/embed/q3UYBirg4U4?rel=0" width="560">
<span>このビデオでは、機械学習環境のセットアップ方法の概要が説明されています。</span>
<param name="movie" value="https://www.youtube.com/embed/q3UYBirg4U4?rel=0">
<param name="allowFullScreen" value="true">
<param name="allowscriptaccess" value="always">
<param name="scale" value="noScale">
</object>
</figure>

既にインスタンスがあるが、まだそれらを Data Science Experience のプロジェクトにリンクしていない場合、以下のステップを実行する必要があります。

1. [IBM Data Science Experience にログオンします](https://datascience.ibm.com)。
2. **「Projects」** > **「View All Projects」**をクリックします。
3. **「Settings」**タブをクリックします。
4. サービスを追加するため、**「Associated Services」**パネルで、**「add associated service」**をクリックし、サービスを選択し、構成情報を入力し、**「Save」**をクリックします。
5. アクセス・トークンを追加するため、以下のステップを実行します。
   6. **「Access Tokens」**パネルで、**「create new token」**をクリックします。
   7. **「Name」**ボックスに名前を入力します。
   8. **「Access Role For Project」**ボックスで、**「Viewer」**または**「Editor」**のいずれかの役割を選択します。
   9. **「Add」**をクリックします。

6. GitHub リポジトリーに接続するため、**「Repository URL」**にリポジトリーの場所を入力します。GitHub アクセス用に既にセットアップされた個人用アクセス・トークンを持っていない場合、**「Settings」**をクリックしてここで作成する必要があります。完了したら**「Add」**をクリックします。


## 資格情報の取得

ノートブックまたはアプリケーションで Bluemix インスタンス、サービス、およびモデルを使用するには、資格情報、トークン、およびスコアリング・エンドポイントを、処理に使用されるコードに挿入できなければなりません。

1. [Bluemix にログインします](https://console.ng.bluemix.net/?cm_sp=dw-bluemix-_-clouddataservices-_-devcenter)。
2. **「サービス」**セクションまでスクロールし、資格情報を取得したいサービスの名前をクリックします。
3. ナビゲーション・ペインで**「サービス資格情報」**をクリックします。
4. 資格情報リストで**「資格情報の表示」**をクリックします。
5. このサービスの資格情報が存在しない場合、**「資格情報の作成 (Create credentials)」**をクリックして作成できます。
6. 資格情報をコピーするには、コピー・アイコンをクリックします。

例えば `username` および `password` など、フィールドはサービスによって異なることがあります。 表示されるとおりの値をコードで使用するには、値をコピーする必要があります。

## 詳細はこちら

[A quick deep learning tutorial](https://www.ibm.com/blogs/watson/2016/10/quick-deep-learning-tutorial/)

