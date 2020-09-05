認証ヘッダ挿入用iRuleの作成
========================================

　ここでは、ユーザ認証ヘッダ（X-Authenticated-User）とAPMで認証されたユーザ名（session.logon.last.username）をICAPリクエストに挿入するためのiRuleを作成します。ICAPはHTTPをカプセリングしますので、認証情報を挿入するためには以下の２つのiRuleが必要となります。また、i-FILTER ICAP版に認証ユーザ名を理解してもらうために、ユーザ名の手前に "ldap:/// " を加え、Base64エンコーディングをする必要があります。

#. 	[Local Traffic] – [iRules] にて、Createボタンを押します。任意の名前を入力して、Definitionに以下サンプルiRuleを入力し、Finishedボタンを押します。（以下のiRuleはあくまでもサンプルとなります。同じ主旨の内容であれば下記と同じでなくても構いません。）

    例１）HTTPリクエストヘッダに認証情報を挿入するためのiRuleサンプル
    .. code-block:: bash

            ###  Add this iRule to Virtual Server:ssloS_XXXX-t-4 ###

            when HTTP_REQUEST {

                # Create the HDR list and insert the X-Server-IP value 
                set hdr [list "X-Server-IP:[IP::local_addr]"]   
    
                # Add the authenticated user information if it exists
                if { [ACCESS::session data get session.logon.last.username] ne "" } {
                    # In case of  NLTM/Basic Auth
                    lappend hdr "X-Authenticated-User:[b64encode ldap:///[ACCESS::session data get session.logon.last.username]]"
                }  

                # Add above info to the table temporarily and add an ICAP-X header 
                set guid [expr {int(rand()*1e12)}] 
                table set ${guid} ${hdr} 10 10 
                HTTP::header replace ICAP-X ${guid} 

            }
    例２）ICAPリクエストヘッダに認証情報を挿入するためのiRuleサンプル
    .. code-block:: bash

            ### Add this iRule to ssloS-XXXX-req ###

            when ICAP_REQUEST {

                if { [HTTP::header exists "ICAP-X"] } {

                    # Get the header and the value from table
                    set hdrs [table lookup [HTTP::header "ICAP-X"]] 
        
                    # Add it to ICAP header 
                    foreach x ${hdrs} {
                    ICAP::header add [lindex [split ${x} ":"] 0] [lindex [split ${x} ":"] 1]
                    }  
        
                # Remove the temporary http header. But the data would show up in the ICAP header.
                HTTP::header remove "ICAP-X" 
                }
            }
    | ２つ目のiRuleにおいて、ICAP-XヘッダはHTTPヘッダ上からは削除されますが、ICAPヘッダに残ります。このヘッダは無害ですが、気になる場合は、i-FILTERのヘッダコントロールにて削除して下さい。
    | また、接続テストをされる際には、変数情報をログ出力するなどして意図する値が入っているか確かめて頂くことをおすすめします。
     
    | 例１）認証ユーザ名を確認するデバッグ
    | log local0. "Username: [ACCESS::session data get session.logon.last.username]"

    | 例２）エンコードされた内容を確認するデバッグ
    | log local0. "Encoded username: b64encode ldap:///[ACCESS::session data get session.logon.last.username]]"

    | 例３）ヘッダー内容を確認するデバッグ
    | foreach attr "[HTTP::header names]" {
    |     log local0. "$attr : [HTTP::header value $attr]"
    | }

