i-FILTERブロック画面のタイトル画像の表示、パスワードブロック解除の設定
====================================================================

　10.3章にて、SSLO連携をするとブロック画面のタイトルが標準では表示されないと記載しましたが、以下の設定を追加することによって、表示させることが可能です。本設定を行うことにより、パスワード入力によるブロック解除も連携が可能となります。
　上記を実現するためには、下図の4つのVirtual Serverが追加で必要となります。ここでは、下図の④②①③の順で作成していきます。

.. image:: images/mod13-1.png
|  

ICAP service request用Virtual Serverの作成
--------------------------------------------

#. まずはICAP用のプロファイルを以下のように作成します。[Local Traffic] – [Profiles] – [Services] – [ICAP]にて、Createボタンを押します。任意の名前を入力し、URIには以下のように入力し、Finishedボタンを押します。
    
    **icap://${SERVER_IP}:${SERVER_PORT}/REQMOD**

    .. image:: images/mod13-2.png
    | 
#. 次にICAP service request 用のVirtual Serverを作成します。[Local Traffic] – [Virtual Servers]にて、Createボタンを押します。任意の名前を入力し、Typeは“Internal”を選択します。

    .. image:: images/mod13-3.png
    | 
#. Configurationにて“Advanced”を選択し、ICAP Profileにて、(1)で作成したICAPプロファイルを選択します。

    .. image:: images/mod13-4.png
    | 
#. 最後に、Default Poolにて、SSLO Guided Configurationで作成済みのPoolを選択して、Finishedボタンを押します。

    .. image:: images/mod13-5.png
    | 

Transparent Proxy用Virtual Serverの作成
--------------------------------------------

#. まず、ICAP用のVirtual Serverを呼び出すためのRequest Adaptプロファイルを作成します。[Local Traffic] – [Profiles] – [Services] – [Request Adapt]にて、Createボタンを押します。任意の名前を入力し、Internal Virtual Nameにて、13.1で作成したVirtual Severを選択し、Preview Sizeにて、”0”を入力し、Finishedボタンを押します。

    .. image:: images/mod13-6.png
    |
#. 次に、SSLサイトにおけるブロック時に、ブロック画面内のタイトル画像にSSL接続するためのClientSSLプロファイルを作成します。[Local Traffic] – [Profiles] – [SSL] – [Client]にて、Createボタンを押します。任意の名前を入力し、タイトル画像をSSL経由で表示させるためのサーバ証明書と秘密鍵を選択し、Finishedボタンを押します。（BIG-IPにてSSLオフロードを行うため、[System] – [Certificate Management] – [Traffic Certificate Management]にて、予めサーバ証明書と秘密鍵の登録が必要となります。）

    .. image:: images/mod13-7.png
    |  
#. 次に、Proxy用のTunnelを作成します。[Network] – [Tunnel List]にて、Createボタンを押します。任意の名前を入力し、Profileにて、”tcp-forward”を選択し、Finishedボタンを押します。

    .. image:: images/mod13-8.png
    |  
#. 次に、HTTPSトラフィックにおけるURI書き換え用のiRuleを作成します。[Local Traffic] – [iRules] にて、Createボタンを押します。任意の名前を入力して、Definitionに以下サンプルiRuleを入力し、Finishedボタンを押します。（以下のiRuleはあくまでもサンプルとなります。同じ主旨の内容であれば下記と同じでなくても構いません。）
    例）URI書き換え用のiRule

    .. code-block:: bash

            ###  Add this iRule to Transparent Virtual Server ###
            when HTTP_REQUEST {
                set OLDURI [HTTP::uri]
                HTTP::uri "https://[HTTP::host]$OLDURI"
            }

#. 次に、SSL用のTransparent Virtual Serverを作成します。[Local Traffic] – [Virtual Servers]にて、Createボタンを押します。任意の名前を入力し、Destination Address/Maskにて、”0.0.0.0/0”を入力、Service Portにて、”15080”と入力します。

    .. image:: images/mod13-9.png
    |  
#. Configurationにて、”Advanced”を選択し、HTTP Profileにて、”http”を選択、SSL Profile (Client)にて、(2)で作成したclientsslプロファイルを選択します。

    .. image:: images/mod13-10.png
    |  
#. Request Adapt Profileにて、(1)で作成したプロファイルを選択し、VLAN and Tunnel Trafficにて、”Enable on...”を選択し、VLANs and Tunnelsにて(3)で作成したTunnelを選択し、Source Address Translationにて”Auto Map”を選択し、Address TranslationとPort Translationのチェックをはずします。

    .. image:: images/mod13-11.png
    |  
#. iRulesにて、4で作成したiRuleを選択し、Finishedボタンを押します。

    .. image:: images/mod13-12.png
    |  

Explicit Proxy用Virtual Serverの作成
--------------------------------------------

#. まず、プロキシ用のExplicit Profileを作成します。[Local Traffic] – [Profiles] – [Services] – [HTTP]にて、createボタンを押します。Proxy Modeにて、”Explicit”を選択します。

    .. image:: images/mod13-13.png
    | 
#. DNS Resolverにて、SSLO Guided Configurationで作成したDNS Resolverを選択し、Tunnel Nameにて、13.2で作成したTunnelを選択し、Finishedボタンを押します。

    .. image:: images/mod13-14.png
    |  
#. 次に、以下の２つのiRuleを作成します。
    * インターネット接続用のHTTP/HTTPsトラフィックとi-FILTERブロックタイトル画面接続トラフィックを分けるiRule
    * 上記後者のトラフィックにおいて、i-FILTER向けにURIを書き換えるiRule
 
    [Local Traffic] – [iRules] にて、Createボタンを押します。任意の名前を入力して、Definitionに以下サンプルiRuleを入力し、Finishedボタンを押します。本iRuleでは、トラフィックの内容をみて、ICAPプロファイルを紐付けるかの判断もしています。また、iRule内のホスト名は、環境にあわせたFQDN/IPアドレスに変更して頂く必要があります。
    （以下のiRuleはあくまでもサンプルとなります。同じ主旨の内容であれば下記と同じでなくても構いません。また、以下の２つのiRuleは、1つのファイルにしていただいても構いません。）
      
    例）トラフィックを分ける用のiRule

    .. code-block:: bash

            ###  Add this iRule to Explicit Virtual Server ###
            when HTTP_PROXY_REQUEST {
                set F5PROXY "bigip.f5jplab.local"
                if { [HTTP::host] contains $F5PROXY} {
                    HTTP::proxy enable
                    ADAPT::enable request false
                } else {
                    HTTP::proxy disable
                    virtual sslo_L3ExplicitProxy.app/sslo_L3ExplicitProxy-xp-4
                    snat automap
                }        
            }
    例）URI書き換え用のiRule

    .. code-block:: bash

        ###  Add this iRule to Explicit Virtual Server ###
        when HTTP_REQUEST {
            set F5PROXY "bigip.f5jplab.local"
            if { [HTTP::host] contains $F5PROXY} {
                set OLDURI [HTTP::uri]
                HTTP::uri "http://[HTTP::host]$OLDURI" 
                ADAPT::enable request true
            } else {
                ADAPT::enable request false
            }
        }

#. 次にExplicit Proxy用のVirtual Serverを作成します。[Local Traffic] – [Virtual Servers]にて、Createボタンを押します。任意の名前を入力し、Destination Address/Maskにて、プロキシ接続用のアドレスを入力、Service Portにて、プロキシとして利用するポート番号を入力します。

    .. image:: images/mod13-15.png
    |  
#. HTTP Profile(Client)にて、1で作成したHTTP Explicit Profileを選択します。

    .. image:: images/mod13-16.png
    |  
#. Request Adapt Profileにて、13.2で作成したプロファイルを選択し、Source Address Translationにて、”Auto Map“を選択します。

    .. image:: images/mod13-17.png
    |  
#. iRulesにて、3にて作成した２つのiRuleを選択して、Finishedボタンを押します。

    .. image:: images/mod13-18.png
    |  
  
Reject用Virtual Serverの作成
-----------------------------------

#. 最後に、15080ポート以外はブロックをするVirtual Serverを作成します。（本Virtual Serverがなくても動作はします。）任意の名前を入力し、Typeにて”Reject”を選択し、Destination Address/Maskにて、”0.0.0.0/0”と入力し、Service Portにて”*”と入力します。VLAN and Tunnel Trafficにて、”Enabled on...”を選択し、VLANs and Tunnelsにて13.2で作成したTunnelを選択し、Finishedボタンを押します。

    .. image:: images/mod13-19.png
    |  

i-FILTER側の設定
-----------------------------------

#. システム／システムパラメーター／機能設定にて、仮想ホスト転送モードを有効にし、コンテンツ転送用アドレスにBIG-IPのFQDNを入力し、ポート番号に”15080”を入力します。（IPアドレスを入力しても動作はしますが、SSL接続時にブラウザでプライバシー保護のエラーが表示されます。）

    .. image:: images/mod13-20.png
    |  
#. システム／システムパラメーター／動作設定にて、コンテンツ転送、ブロック解除のそれぞれに任意の値を設定します。（ブロック画面タイトル表示時、パスワードブロック解除時のURIの一部として利用されます。）

    .. image:: images/mod13-21.png
    |  

クライアントからの接続テスト
-----------------------------------

#. ブラウザのプロキシ設定にて、13.３で作成したExplicit ProxyのIPアドレスに紐づくFQDNまたは、IPアドレスに変更します。
    |  
#. ブラックリストの宛先への通信がブロックされ、タイトル画像が表示されることを確認します。

    .. image:: images/mod13-22.png
    |  
#. パスワードブロック解除が設定されている宛先へ接続し、パスワード入力画面が表示されることを確認します。

    .. image:: images/mod13-23.png
    |  
#. 解除パスワード入力後、無事WEB接続ができる事を確認します。

    .. image:: images/mod13-24.png
    |  

