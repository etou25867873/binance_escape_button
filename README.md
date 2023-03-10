## 環境構築
rubyバージョン:3.2.0

rubyインストール時に、「cannot load such file -- psych」エラーが発生した場合、下記URLをご参考ください。

https://zenn.dev/fuuukeee3/articles/22caeb537cf1a6

rubyコマンドの参照先が切り替わらない場合、下記URLをご参考ください。

https://qiita.com/opiyo_taku/items/3312a75d5916f6cd32b1

## Appの入り口

http://localhost:3000

## 事前準備
1. BinanceでAPIを登録。
   - HMAC認証のAPIを登録。
   - 引き出し権限をAPIに付与するため、Appを実行するPublic IPをAPIのIP制限を追加。
2. 通貨毎に引き出し先のアドレスをBinanceのホワイトリストに登録。
3. .envファイル作成
   - ルートディレクトリに.envファイルを作成
   - Step 1で登録したAPIキーと秘密鍵を以下のように.envファイルに記載。
      ```
      BINANCE_API_KEY="APIキー"
      BINANCE_SECRET_KEY="秘密鍵"
      ```
   - Step 2で登録したアドレスとそのアドレスが属するネットワークを以下のように通貨毎に.envファイルに記載。キーのプリフィックスはTickerの大文字。
      ```
      NEAR_WALLET="アドレス"
      NEAR_NETWORK="ネットワーク"
      ```
      ネットワークの値の取得
      
      方法1:
      下記Address Management画面のNetworkの列より取得できる。
      
      https://www.binance.com/en/my/security/address-management
      
      方法2:
      [All Coins' Information API](#all-coins-information-user_data)より取得できる。
   - APIキー、秘密鍵、アドレス、ネットワークを.envファイルに記載しない場合、「Escape」ボタンが非表示となり、引き出しができない。

## 操作方法
1. 「Show Configuration & System Status & Balance」ボタンを押して、APIキー、秘密鍵、通貨別のアドレスとネットワークを表示。
2. 「Escape」ボタンを押して、該当する通貨の残高を引き出す。
3. 通貨のwithdrawIntegerMultipleによって引き出し金額の端数を切り捨て。引き出し最小金額withdrawMinより少ない場合、エラーになる。
   withdrawIntegerMultiple、withdrawMinについては[All Coins' Information API](#all-coins-information-user_data)にご参照ください。

   
## All Coins' Information (USER_DATA)
```
GET /sapi/v1/capital/config/getall (HMAC SHA256)
```
API Document: https://binance-docs.github.io/apidocs/spot/en/#all-coins-39-information-user_data
