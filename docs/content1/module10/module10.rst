SSLO Guided ConfigurationによるSSLOの設定
================================================

#. [SSL Orchestrator] – [Configuration]を選択します。DNSとNTPとRouteがConfigureとなっているのを確認し、Nextボタンを押します。

    .. image:: images/mod10-1.png
    |  
#. 任意の名前を設定し、SSL Orchestrator Topologiesとして、”L3 Explicit Proxy”を選択し、Save&Nextボタンを押します。

    .. image:: images/mod10-2.png
    |  
#. 任意の名前を設定し、右上のShow Advanced Settingをクリックし、Client-side SSLにて、利用したいTLSのバージョンを選択します。

    .. image:: images/mod10-3.png
    |  
#. CA Certificate KeyChainにて、既にインポート済みのCAファイルを選択してDoneを押します。

    .. image:: images/mod10-4.png
    |  
#. Server-side SSLも同様に利用したいバージョンを選択します。

    .. image:: images/mod10-5.png
    |  
#. 期限切れの証明書や自己署名証明書に対しての動作も確認し、Save&Nextを押します。

    .. image:: images/mod10-6.png
    |  
#. Add Serviceを押します。

    .. image:: images/mod10-7.png
    |  
#. Generic ICAP Serviceを選択し、Addボタンを押します。

    .. image:: images/mod10-8.png
    |  
#. 任意の名前を設定し、ICAP Devicesにi-FILTERのIPアドレスとポート番号を設定し、Doneを押します。

    .. image:: images/mod10-9.png
    |  
#. Request Modification URI Pathに”/REQMOD”を入力し、Preview Max Lengthに”0”を入力し、ICAP Policyに既に作成済みのLocal Traffic Policyを選択し、Saveを押します。（i-FILTER ICAP版は今日現在ICAP Previewに対応していないので、０を入力します。）

    .. image:: images/mod10-10.png
    |  
#. Save&Nextを選択します。

    .. image:: images/mod10-11.png
    |  
#. Service Chain ListでAddを押します。

    .. image:: images/mod10-12.png
    |  
#. 任意の名前を設定し、先程作成したサービスを右に移動させ、Saveボタンを押します。

    .. image:: images/mod10-13.png
    |  
#. Service Chainができたことを確認し、Save&Nextボタンを押します。

    .. image:: images/mod10-14.png
    |  
#. All Trafficのペンマークをクリックします。

    .. image:: images/mod10-15.png
    |  
#. 先程作成したService Chainを選択し、OKボタンを押します。

    .. image:: images/mod10-16.png
    |  
#. Save&Nextボタンを押します。

    .. image:: images/mod10-17.png
    |  
#. Proxy Server SettingsにクライアントからプロキシとしてアクセスさせるIPアドレスを入力し、既に作成済みのAccess Profileを選択し、Ingress Networkとして、クライアントからアクセス可能なVLANを選択し、Save&Nextボタンを押します。

    .. image:: images/mod10-18.png
    |  
#. 本テスト構成では、Manage SNAT SettingsでAuto Map、GatewaysでDefault Routeを選択し、Save＆Nextボタンを押します。(設定は検証環境に合わせてください。)

    .. image:: images/mod10-19.png
    |  
#. Save&Nextボタンを押します。

    .. image:: images/mod10-20.png
    |  
#. 必要に応じて、設定内容を見直し、Save&Nextボタンを押します。

    .. image:: images/mod10-21.png
    |  
#. OKボタンを押し、Deployに成功すると以下のような緑色のDEPLOYEDマークが表示されます。

    .. image:: images/mod10-22.png
    |  
#. 右上のSystem Settingsアイコンを選択します。

    .. image:: images/mod10-23.png
    |  
#. SSLOがExplicit Proxyとして利用するDNSを設定し、Deployを押します。

    .. image:: images/mod10-24.png
    |  
#. i-FILTER ICAP版との連携はICAPリクエストのみ設定をすればよいので、ICAPレスポンスの設定を外します。SSLO標準では、ICAPレスポンスは必須となっているので、Protectedを外してから以下の操作をします。本設定をしないと/var/log/ltmに”Unexpected status code 501 received from server”といったエラーメッセージが表示されます。
Servicesにおいて、作成済みのService（ICAPのサービス）を選択し、Protectedの鍵マークを外します。

    .. image:: images/mod10-25.png
    |  
#. [Local Traffic] – [Virtual Servers] において、ssloS_XXXX(任意)-t-4という名称のVirtual Serverを選択し、ConfigurationにてAdvancedを選択し、Response Adapt ProfileにてNoneを選択し、Updateボタンを押します。

    .. image:: images/mod10-26.png
    |  
#. 次に既に作成済みのiRuleを追加します。[Local Traffic] – [Virtual Servers] において、ssloS_XXXX(任意)-t-4という名称のVirtual Serverを選択し、Resourcesタブを選択、HTTPリクエストヘッダに認証情報を挿入するためのiRuleを選択し、Finishedを押します。 


    .. image:: images/mod10-27.png
    |  
#. 次にICAPリクエストヘッダに加えるためのiRuleを追加します。[Local Traffic] – [Virtual Servers] において、ssloS_XXXX(任意)-reqという名称のVirtual Serverを選択し、Resourcesタブを選択、ICAPリクエストヘッダに認証情報を挿入するためのiRuleを選択し、Finishedを押します。


    .. image:: images/mod10-28.png
    |  