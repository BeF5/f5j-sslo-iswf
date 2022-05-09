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

    * SSL可視化バイパス設定を行っているサイトでは、InterSafeの規制画面が表示されない場合があります。  
        規制画面を表示する必要があるサイトについては、SSLO上でのパイパス設定を行わないでください。
    
    * 本ガイドでは、Webで利用するデフォルトのポート番号に関してInterSafeポート規制機能との
        連携方法を記載しています。
        記載外のポートに対して、ポート規制機能を利用する場合、ガイド内1.7の設定を
        必要なポートに対して追加してください。
    
    * InterSafe認証機能については、以下機能と連携可能であることを確認しています。
        IPアドレス認証
        Basic認証(ローカル)
        Basic認証(LDAP連携)
