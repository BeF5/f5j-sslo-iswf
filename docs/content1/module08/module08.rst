NTLM認証設定
====================

　APMで行う認証の設定を行います。ここでは、NTLM認証の設定を行います。ADサーバ（ドメインコントローラ、DNS）の構築済みであることを前提としております。

#. **Access >> Authentication >> NTLM >> Machine Account** を選択し、:guilabel:`Create` ボタンを押します。

    .. image:: images/mod8-1.png
    |  
#. **任意の名前** を設定し、**Machine Account Name** には **任意のSSLO識別名** を入力し、**Domain FQDN**、**Adminユーザ名** と **パスワード** を入力し、:guilabel:`Join` を押します。

    .. image:: images/mod8-2.png
    |  
#. 以下のようになります。

    .. image:: images/mod8-3.png
    |  
#. **Access >> Authentication >> NTLM >> NTLM Auth Configuration** を選択し、:guilabel:`Create` ボタンを押します。

    .. image:: images/mod8-4.png
    |  
#. **任意の名前** を設定し、**Machine Account Name** は **先程作成したもの** を選択します。**Domain Controller FQDN List** には **ご利用のドメイン名（FQDN名）** を指定し、:guilabel:`Finishedボタン` を押します。

    .. image:: images/mod8-5.png
    |  
#. **Access >> Profiles/Policies >> Access Profiles(Per-Session Policies)** を選択し、:guilabel:`Create` ボタンを押します。

    .. image:: images/mod8-6.png
    |  
#. **任意の名前** を設定し、**Policy Type** に **SWG-Explicit** を選択し、**Customization Type** に **Standard** を選択し、**User Identification Method** にて、**Credential** を選択し、**NTLM Auth Configuration** に **先程作成したもの** を選択し、**Languages** は **ご利用の言語** を選択して、:guilabel:`Finished` ボタンを押します。

    .. image:: images/mod8-7.png
    |  
#. 作成されたAccess Profileの一覧から先程作成したプロファイルを見つけ、Per-Session Policy欄の :guilabel:`Edit` を押します。 

    .. image:: images/mod8-8.png
    |  
#. ブラウザの別タブにVPEが表示されます。Startの右隣の :guilabel:`＋` ボタンを押します。

    .. image:: images/mod8-9.png
    |  
#. Authenticationタブの **NTLM Auth Result** を選択し、:guilabel:`Add Item`、:guilabel:`Save` を押します。

    .. image:: images/mod8-10.png
    |  
#. NTLM Auth Resultの右のSuccessfulにつながるフローを **Deny** から **Allow** に変更します。

    .. image:: images/mod8-11.png
    |  
#. 左上の :guilabel:`Apply Access Policy` を押し、ブラウザのVPEタブを閉じます。

    .. image:: images/mod8-12.png
    |  
