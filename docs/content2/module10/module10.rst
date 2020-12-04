SSLO Guided ConfigurationによるSSLOの設定
================================================

#. **SSL Orchestrator >> Configuration** を選択します。**DNS** と **NTP** と **Route** が **Configure** となっているのを確認し、:guilabel:`Next` ボタンを押します。

    .. image:: images/mod10-1.png
    |  
#. **任意の名前** を設定し、SSL Orchestrator Topologiesとして、**L3 Explicit Proxy** を選択し、:guilabel:`Save＆Next` ボタンを押します。

    .. image:: images/mod10-2.png
    |  
#. **任意の名前** を設定し、右上の :guilabel:`Show Advanced Setting` をクリックし、**Client-side SSL** にて、利用したい **TLSのバージョン** を選択します。

    .. image:: images/mod10-3.png
    |  
#. **CA Certificate KeyChain** にて、既にインポート済みのCAファイルを選択して :guilabel:`Done` を押します。

    .. image:: images/mod10-4.png
    |  
#. **Server-side SSL** も同様に利用したい **TLSバージョン** を選択します。

    .. image:: images/mod10-5.png
    |  
#. 期限切れの証明書や自己署名証明書に対しての動作も確認し、:guilabel:`Save＆Next` を押します。

    .. image:: images/mod10-6.png
    |  
#. :guilabel:`Add Service` を押します。

    .. image:: images/mod10-7.png
    |  
#. **Generic HTTP Service** を選択し、:guilabel:`Add` ボタンを押します。

    .. image:: images/mod10-8.png
    |  
#. **任意の名前** を設定します。

    .. image:: images/mod10-9.png
    |  
#. **Service Definition** にて、**Auto Manage Addresses** のチェックをはずし、SSLOからi-FILTER ProxyへHTTPトラフィックを転送するVLAN, Self IPの情報を入力します。 （ここでは、検証環境の都合でAuto Manage Addressesは利用しておりません。）

    .. image:: images/mod10-10.png
    |  
    .. note::
        - SSL可視化ゾーン（ここではi-FILTER Proxy版）では、HTTPトラフィックは暗号化されておりませんので、第三者からのアクセスを避ける必要があります。そこで、SSLOでは、Auto Manage Addresses（RFC2544に定められているインターナル利用アドレス）をセキュリティ機器に利用頂くことを推奨しております。
#. i-FILTER Proxyのインライン側の **IPアドレス** と **ポート番号** を入力し、:guilabel:`Done` を押します。

    .. image:: images/mod10-11.png
    |  
#. i-FILTER ProxyからSSLOへHTTPトラフィックを転送するVLAN, Self IPの情報を入力します。

    .. image:: images/mod10-12.png
    |  
#.  検証環境では **Auto Map** を設定し、作成済みのiRuleを右に移動させ、:guilabel:`Save` を押します。　

    .. image:: images/mod10-13.png
    |  
#. Serviceが追加されるとこのような画面になります。:guilabel:`Save＆Next` を押します。　

    .. image:: images/mod10-14.png
    |  
#. :guilabel:`Add` をクリックし、サービスチェーンを作成します。

    .. image:: images/mod10-15.png
    |  
#. **任意の名前** を設定し、作成済みのServiceを右に移動させ、:guilabel:`Save` ボタンを押します。

    .. image:: images/mod10-16.png
    |  
#. サービスチェーンが作成されると、このような画面になります。:guilabel:`Save＆Next` ボタンを押します。

    .. image:: images/mod10-17.png
    |  
#. All Trafficeの :guilabel:`ペンマーク` をクリックします。

    .. image:: images/mod10-18.png
    |  
#. 先程作成した Service Chain を選択し、:guilabel:`OK` ボタンを押します。

    .. image:: images/mod10-19.png
    |  
#. All TrafficeにService Chainが追加されると以下のような画面になります。

    .. image:: images/mod10-20.png
    |  
#. :guilabel:`Save & Next` ボタンを押します。

    .. image:: images/mod10-21.png
    |  
#. Proxy Server Settings にクライアントからプロキシとしてアクセスさせるIPアドレスを入力し、既に作成済みの Access Profile を選択し（プロキシ認証しない場合は不要）、Ingress Network として、クライアントからアクセス可能な VLAN を選択し、:guilabel:`Save＆Next` ボタンを押します。

    .. image:: images/mod10-22.png
    |  
#. 本テスト構成では、Manage SNAT Settings で Auto Map、Gateways で Default Route を選択し、:guilabel:`Save＆Next` ボタンを押します。(設定は検証環境に合わせてください。)

    .. image:: images/mod10-23.png
    |  
#. :guilabel:`Save＆Next` ボタンを押します。

    .. image:: images/mod10-24.png
    |  
#. 必要に応じて、設定内容を見直し、:guilabel:`Save＆Next` ボタンを押します。

    .. image:: images/mod10-25.png
    |  
#. :guilabel:`OK` ボタンを押し、Deployに成功すると以下のような緑色の **DEPLOYED** マークが表示されます。

    .. image:: images/mod10-26.png
    |  
#. 右上の **System Settings** アイコンを選択します。

    .. image:: images/mod10-27.png
    |  
#. SSLOがExplicit Proxyとして利用する **DNS** を設定し、:guilabel:`Deploy` を押します。

    .. image:: images/mod10-28.png
    |  

.. note::
    - セキュリティデバイスがICAPサービス、HTTPサービスの場合、SSL復号していないトラフィックをサービスチェーンに流せません。