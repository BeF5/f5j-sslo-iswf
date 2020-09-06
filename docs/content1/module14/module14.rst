その他
==========================

各種ログ
--------------------------------------------

#. テストの際には、以下のSSLOにおけるログを参照して下さい。
    * SSLO全般ログやiRuleでlocal0で指定したログ
        /var/log/ltm

    * APM（認証）のログ
        /var/log/apm

    * SSL Guided Configuration（iAppLX/REST）に関わるログ
        /var/log/restnoded/restnoded.log
        /var/log/restjava.0.log
    
    * その他、SSLOに関するトラブルシューティングは以下のAskF5記事をご参照下さい。
        K26520133: Troubleshooting SSL Orchestrator(SSLO)
        https://support.f5.com/csp/article/K26520133
    |  
#. テストの際には、以下のi-FILTERにおけるログを参照して下さい。
    * アクセスログ
        /usr/local/ifilter10/logs/access.log0000
    |  

フィルター除外設定（必須ではございません）
--------------------------------------------

#. 本テストでは、httpbin.org を接続先ホストとして利用させて頂いておりますが、現状デフォルトではブロックされるので、**システム／システムフィルター／推奨フィルター設定 – 除外設定** にて、テストで利用しているホスト名を **除外設定** に追加します。

    .. image:: images/mod14-1.png
    |  




