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
#. **Generic ICAP Service** を選択し、:guilabel:`Add` ボタンを押します。

    .. image:: images/mod10-8.png
    |  
#. **任意の名前** を設定し、**ICAP Devices** に **InterSafe WebFilterのIPアドレス** と **ポート番号** を設定し、:guilabel:`Done` を押します。

    .. image:: images/mod10-9.png
    |  
#. **Request Modification URI Path** に **/REQMOD** を入力し、**Preview Max Length** に **0** を入力し、**ICAP Policy** に既に作成済みの **Local Traffic Policy** を選択し、:guilabel:`Save` を押します。（InterSafe WebFilter ICAP版は **ICAP Preview** に対応していないので、**０** を入力します。）

    .. image:: images/mod10-10.png
    |  
#. **Service Chain List** で :guilabel:`Add` を押します。

    .. image:: images/mod10-16.png
    |  
#. **任意の名前** を設定し、先程作成したサービスを右に移動させ、:guilabel:`Save` ボタンを押します。(InterSafe WebFilterのサービスが上にくるようにします。)

    .. image:: images/mod10-17.png
    |  
#. **Service Chain** ができたことを確認し、:guilabel:`Save＆Next` ボタンを押します。

    .. image:: images/mod10-18.png
    |  
#. All Trafficの :guilabel:`ペンマーク` をクリックします。

    .. image:: images/mod10-19.png
    |  
#. 先程作成した **Service Chain** を選択し、:guilabel:`OK` ボタンを押します。

    .. image:: images/mod10-20.png
    |  
#. :guilabel:`Save＆Next` ボタンを押します。

    .. image:: images/mod10-21.png
    |  
#. **Proxy Server Settings** にクライアントからプロキシとしてアクセスさせるIPアドレスを入力し、既に作成済みの **Access Profile** を選択し、**Ingress Network** として、クライアントからアクセス可能な **VLAN** を選択し、:guilabel:`Save＆Next` ボタンを押します。

    .. image:: images/mod10-22.png
    |  
#. 本テスト構成では、**Manage SNAT Settings** で **Auto Map**、**Gateways** で **Default Route** を選択し、:guilabel:`Save＆Next` ボタンを押します。(設定は検証環境に合わせてください。)

    .. image:: images/mod10-23.png
    |  
#. :guilabel:`Save＆Next` ボタンを押します。

    .. image:: images/mod10-24.png
    |  
#. 必要に応じて、設定内容を見直し、 :guilabel:`Deploy` ボタンを押します。

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
#. InterSafe WebFilterと連携するサービスに関しては認証ヘッダを挿入するために、Protectedの **鍵** マークを外します。

    .. image:: images/mod10-29.png
    |  
#. 既に作成済みの **iRule** を追加します。**Local Traffic >> Virtual Servers>>** において、**ssloS_XXXX(任意)-t-4** という名称のVirtual Serverを選択し、**Resources** タブを選択、HTTPリクエストヘッダに認証情報を挿入するための **iRule** を選択し、:guilabel:`Finished` を押します。 

    .. image:: images/mod10-31.png
    |  
#. ICAPリクエストヘッダに加えるための **iRule** を追加します。**Local Traffic >> Virtual Servers** において、**ssloS_XXXX(任意)-req** という名称のVirtual Serverを選択し、**Resources** タブを選択、ICAPリクエストヘッダに認証情報を挿入するための **iRule** を選択し、:guilabel:`Finished` を押します。

    .. image:: images/mod10-32.png
    |  
    
.. note::
    - セキュリティデバイスがICAPサービス、HTTPサービスの場合、SSL復号していないトラフィックをサービスチェーンに流せません。