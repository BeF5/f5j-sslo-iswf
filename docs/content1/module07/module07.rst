i-FILTERのHTTP/HTTPS判別用DBの対応設定（Local Traffic Policyの設定）
==================================================================

　i-FILTER ICAP版は、HTTPのリクエストヘッダの一部でHTTPサーバへの通信かHTTPSサーバへの通信かを判別しています。i-FILTERがHTTP/HTTPS判別可能となるようにLocal Traffic Policyにてルールを作成します。

#. **Local Traffic >> Policies >> Policies List** にて、:guilabel:`Create` ボタンを押します。

    .. image:: images/mod7-1.png
    |  
#. **任意のポリシー名** を入力し、:guilabel:`Create Policy` ボタンを押します。

    .. image:: images/mod7-2.png
    |  
#. HTTPS用のルールを作成します。Rulesの :guilabel:`Create` ボタンを押します。

    .. image:: images/mod7-3.png
    |  
#. **任意のRule名** を入力し、**Match all of the following conditions:** の :guilabel:`＋` マークをクリックし、以下のように入力します。

    .. image:: images/mod7-4.png
    |  
    .. csv-table:: 
         :header: "Match all of the following conditions:", "必要有無"
         :widths: 50, 5

         "**TCP port is any of 443** at", "必須"
         "**client accepted** time.", "必須"
         "Apply to traffic on **local** side of **external** interface","必須"
    |  
#. 同様に、**Do the following when the traffic is matched:** の :guilabel:`＋` マークをクリックし、以下のように入力し、:guilabel:`Save` ボタンを押します。（デバック用のログルールは任意で追加します。）

    .. image:: images/mod7-5.png
    |  
    .. csv-table:: 
         :header: "Do the following when the traffic is matched:", "必要有無"
         :widths: 95, 5

         "**Replace HTTP URI full string** with value **tcl:https://[HTTP::host][HTTP::uri]** at **request** time.", "必須"
         "**Log** message **tcl: HTTPs(443) URI was replaced to: [HTTP::uri]** at **request** time.", "任意"
         "Facility: **local0** Priority: **info**","任意" 
    |  
#. 同様にHTTP用のルールを作成します。

    .. image:: images/mod7-6.png
    |  
    .. csv-table:: 
         :header: "Match all of the following conditions:", "必要有無"
         :widths: 55, 5

         "**TCP port is any of 80** at", "必須"
         "**client accepted** time.", "必須"
         "Apply to traffic on **local** side of **external** interface","必須"
    .. csv-table:: 
         :header: "Do the following when the traffic is matched:", "必要有無"
         :widths: 55, 5

         "**Replace HTTP URI full string** with value **tcl:http://[HTTP::host][HTTP::uri]** at **request** time.", "必須"
         "Log message tcl: HTTP(80) URI was replaced to: [HTTP::uri] at request time.", "任意"
         "Facility: **local0** Priority: **info**","任意"      
    |  
#. ２つのルール作成後は、以下のようになります。:guilabel:`Save Draft` ボタンを押します。

    .. image:: images/mod7-7.png
    |  
#. :guilabel:`Publish` ボタンを押すと、以下のようになります。

    .. image:: images/mod7-8.png
    |  
    
