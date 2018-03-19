The Python Console
==================
Python コンソール
================

Most Electrum commands are available not only using the command-line,
but also in the GUI Python console. 

ほとんどのELectrumコマンドはコマンドラインだけでなくGUI Pythonコンソールでも使用できます。

The results are Python objects, even though they are sometimes
rendered as JSON for clarity.

わかりやすくするためにJSONで表示されることがありますが結果はPythonオブジェクトです。

Let us call listunspent(), to see the list of unspent outputs in the
wallet:

listunspent()を呼び出して、ウォレット内の未使用アウトプットの一覧を確認しましょう。

.. code-block:: python

   >> listunspent()
   [
    {
        "address": "12cmY5RHRgx8KkUKASDcDYRotget9FNso3",
        "index": 0,
        "raw_output_script": "76a91411bbdc6e3a27c44644d83f783ca7df3bdc2778e688ac",
        "tx_hash": "e7029df9ac8735b04e8e957d0ce73987b5c9c5e920ec4a445130cdeca654f096",
        "value": 0.01
    },
    {
        "address": "1GavSCND6TB7HuCnJSTEbHEmCctNGeJwXF",
        "index": 0,
        "raw_output_script": "76a914aaf437e25805f288141bfcdc27887ee5492bd13188ac",
        "tx_hash": "b30edf57ca2a31560b5b6e8dfe567734eb9f7d3259bb334653276efe520735df",
        "value": 9.04735316
    }
   ]

Note that the result is rendered as JSON.  However, if we save it to a
Python variable, it is rendered as a Python object:

結果はJSONとして表示されることに注意してください。しかし、Python変数に保存すると、Pythonオブジェクトとして表示されます。

.. code-block:: python

   >> u = listunspent()
   >> u 
   [{'tx_hash': u'e7029df9ac8735b04e8e957d0ce73987b5c9c5e920ec4a445130cdeca654f096', 'index': 0, 'raw_output_script': '76a91411bbdc6e3a27c44644d83f783ca7df3bdc2778e688ac', 'value': 0.01, 'address': '12cmY5RHRgx8KkUKASDcDYRotget9FNso3'}, {'tx_hash': u'b30edf57ca2a31560b5b6e8dfe567734eb9f7d3259bb334653276efe520735df', 'index': 0, 'raw_output_script': '76a914aaf437e25805f288141bfcdc27887ee5492bd13188ac', 'value': 9.04735316, 'address': '1GavSCND6TB7HuCnJSTEbHEmCctNGeJwXF'}]

This makes it possible to combine Electrum commands with Python. For
example, let us pick only the addresses in the previous result:

これにより、ElectrumコマンドとPythonを組み合わせることができます。たとえば、先ほどの結果中のアドレスのみを選択します。

.. code-block:: python

   >> map(lambda x:x.get('address'), listunspent())
   [
    "12cmY5RHRgx8KkUKASDcDYRotget9FNso3",
    "1GavSCND6TB7HuCnJSTEbHEmCctNGeJwXF"
   ]

Here we combine two commands, listunspent and dumpprivkeys, in order
to dump the private keys of all adresses that have unspent outputs:

ここでは、未使用アウトプットを持つすべてのアドレスの秘密鍵をダンプするために、listunspentとdumpprivkeysの2つのコマンドを組み合わせています:

.. code-block:: python

   >> dumpprivkeys( map(lambda x:x.get('address'), listunspent()) )
   {
    "12cmY5RHRgx8KkUKASDcDYRotget9FNso3": "***************************************************",
    "1GavSCND6TB7HuCnJSTEbHEmCctNGeJwXF": "***************************************************"
   }

Note that dumpprivkey will ask for your password if your
wallet is encrypted.
The GUI methods can be accessed through the gui variable.
For example, you can display a QR code from a string
using gui.show_qrcode. Example:

ウォレットが暗号化されている場合、dumpprivkeyにはパスワードが必要です。GUIメソッドはgui変数から利用できます。たとえば、gui.show_qrcodeを使用して文字列からQRコードを表示することができます。例：

.. code-block:: python

   gui.show_qrcode(dumpprivkey(listunspent()[0]['address']))
