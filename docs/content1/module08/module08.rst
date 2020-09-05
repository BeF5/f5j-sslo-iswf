NTLM認証設定
====================

　APMで行う認証の設定を行います。ここでは、NTLM認証の設定を行います。ADサーバ（ドメインコントローラ、DNS）の構築済みであることを前提としております。

#. 	[Access] – [Authentication] – [NTLM] – [Machine Account]を選択し、Createボタンを押します。

    .. image:: images/mod8-1.png
    |  
#. 任意の名前を設定し、Machine Account Nameには任意のSSLO識別名を入力し、Domain FQDN、Adminユーザ名とパスワードを入力し、Joinを押します。

    .. image:: images/mod8-2.png
    |  
#. 以下のようになります。

    .. image:: images/mod8-3.png
    |  
#. [Access] – [Authentication] – [NTLM] – [NTLM Auth Configuration]を選択し、Createボタンを押します。

    .. image:: images/mod8-4.png
    |  
#. 任意の名前を設定し、Machine Account Nameは先程作成したものを選択します。Domain Controller FQDN Listにはご利用のドメイン名（FQDN名）を指定し、Finishedボタンを押します。

    .. image:: images/mod8-5.png
    |  
#. [Access] – [Profiles/Policies] – [Access Profiles(Per-Session Policies)]を選択し、Createボタンを押します。

    .. image:: images/mod8-6.png
    |  
#. 任意の名前を設定し、Policy Typeに”SWG-Explicit”を選択し、Customization Typeに”Standard を選択し、”User Identification Methodにて、”Credential”を選択し、NTLM Auth Configurationに先程作成したものを選択し、Languagesはご利用の言語を選択して、Finishedボタンを押します。

    .. image:: images/mod8-7.png
    |  
#. 作成されたAccess Profileの一覧から先程作成したプロファイルを見つけ、Per-Session Policy欄のEditを押します。 

    .. image:: images/mod8-8.png
    |  
#. ブラウザの別タブにVPEが表示されます。Startの右隣の＋ボタンを押します。

    .. image:: images/mod8-9.png
    |  
#. AuthenticationタブのNTLM Auth Resultを選択し、Add Item、Saveを押します。

    .. image:: images/mod8-10.png
    |  
#. NTLM Auth Resultの右のSuccessfulにつながるフローをDenyからAllowに変更します。

    .. image:: images/mod8-11.png
    |  
#. 左上のApply Access Policyを押し、ブラウザのVPEタブを閉じます。

    .. image:: images/mod8-12.png
    |  
