認証ヘッダ挿入用iRuleの作成
========================================

　ここでは、ユーザ認証ヘッダ（Proxy-Authorization）とAPMで認証されたユーザ名（session.logon.last.username）をHTTPリクエストに挿入するためのiRuleを作成します。i-FILTER PROXY版連携では、**Proxy-Authorization** というヘッダー名に、ユーザ名をBase64エンコーディングしたBasic認証の形式で挿入します。

#. **Local Traffic >> iRules** にて、2- 2.7で作成済みのiRuleにユーザ名用のヘッダーを挿入するための箇所を追記します。（以下のiRuleはあくまでもサンプルとなります。同じ主旨の内容であれば下記と同じでなくても構いません。）

    例）HTTPリクエストにi-FILTER連携のヘッダを挿入するためのiRuleサンプル 
    .. code-block:: bash

            ###  Add this iRule to Virtual Server:ssloS_XXXX-t-4 ###

            when HTTP_REQUEST {

                if { [TCP::local_port] == 80 } {
                    # For HTTP protocol
                    HTTP::header replace "X-Forwarded-Proto" "http"     

                } elseif { [TCP::local_port] == 443 } {
                    # For HTTPS protocl
                    HTTP::header replace "X-Forwarded-Proto" "https"
                } 
    
                sharedvar ctx
                # For Client IP address
                lappend ctx(headers) "X-Forwarded-For" [IP::client_addr]
                
                # For Username
                if { [ACCESS::session data get session.logon.last.username] ne "" } {
                    set PROXYAUTH "Basic [b64encode [ACCESS::session data get session.logon.last.username]:]"

                } else {
                    set PROXYAUTH ""
                }
                lappend ctx(headers) "Proxy-Authorization" $PROXYAUTH

                if { [expr { [llength $ctx(headers)] % 2 }] == 0 } { 
                    foreach {h_name h_value} $ctx(headers) {
                    HTTP::header replace ${h_name} ${h_value} 

                }
            } 

    | 接続テストをされる際には、変数情報をログ出力するなどして意図する値が入っているか確かめて頂くことをおすすめします。
     
    | 例１）認証ユーザ名を確認するデバッグ
    | log local0. "Username: [ACCESS::session data get session.logon.last.username]"

    | 例２）エンコードされた内容を確認するデバッグ
    | log local0. "Encoded username: b64encode [ACCESS::session data get session.logon.last.username]"

    | 例３）ヘッダー内容を確認するデバッグ
    | foreach attr "[HTTP::header names]" {
    |     log local0. "$attr : [HTTP::header value $attr]"
    | }

