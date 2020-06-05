# README
![logo](https://user-images.githubusercontent.com/62282502/83133907-83c46b00-a11e-11ea-900c-3a7130600928.png)

プログラミングスクールTECH::CAMPの最終課題にて某フリーマーケットサービスのクローンサイトを作成しました。
約1ヶ月間、４人チームでアジャイル開発を行いました。

## 🌐 App URL

### **http://54.168.43.9**
※Basic認証をかけています。ご覧の際は以下のIDとPassを入力してください。

- Basic認証
  - ID: Yuto-chan
  - Pass: ppf5p3d

**Chromeのバージョンアップデートでセキュリティ強化された為、現状Basic認証が度々求められる仕様となっています。そのため、ブラウザはSafariにて閲覧ください。**

## テストユーザー
- **購入者用アカウント**
  - メールアドレス: mayumi@gmail.com
  - パスワード: mayumayu
- **購入用カード情報**
  - 番号： 4242424242424242
  - 期限： 7/25
  - セキュリティコード：123
- **出品者用アカウント**
  - メールアドレス名: test@gmail.com
  - パスワード: testtest
　
## 🔨開発環境

- Ruby 2.5.1
- Rails 5.2.4.2
- MySQL
- Haml/SCSS
- jQuery
- VSCode（Visual Studio Code）
- AWS(EC2/E3)
- Github

##Yuto-chanの担当箇所
###デプロイ担当
•アプリを本番環境にアップロードするデプロイの担当を担っていました
###カテゴリー機能(フロントエンド、バックエンド)
•カテゴリー欄にカーソルを乗せた時に、親/子/孫カテゴリーを順に表示できるようにしました
https://gyazo.com/bde52283246871fb16e2ae011b5ff8e9
•商品出品時に、Ajaxを使った非同期通信でカテゴリーを選択できるようにしました。
https://gyazo.com/355d96d85d655488ee56a4f5cad65304
•商品情報のカテゴリー欄に商品のカテゴリー情報が表示されるように実装していますトップページ
https://gyazo.com/28751de76de23830fb1be097214f2345
###商品詳細ページ（フロントエンド）
・自分、または他ユーザーが投稿した商品の詳細を閲覧できるようにしました
https://gyazo.com/7a2c855d133c94da08b7a27f27a87b4d
###商品出品機能（フロントエンド）
•出品する商品の情報を入力することで、自分の商品を登録することができます
https://gyazo.com/34e2f127dc0b17687b3d50eddb69951d

# フリマアプリDB設計
## usersテーブル
|Column|Type|Options|
|------|----|-------|
|nickname|string|null: false|
|email|string|null: false, unique: true|
|password|string|nill: false|
|firstname|string|null:false|
|lastname|string|null: false|
|firstname_kana|string|null: false|
|lastname_kana|string|null: false|
|birth_year|integer|null: false|
|birth_month|integer|null: false|
|birth_day|integer|null: false|
### Association
- has_many :items, dependent: :destroy
- has_many :comments, dependent: :destroy
- has_many :likes, dependent: :destroy
- has_one :profile, dependent: :destroy
- has_many :addresses, dependent: :destroy
- has_one :credit_card, dependent: :destroy

## profilesテーブル
|Column|Type|Options|
|------|----|-------|
|profile_sentence|text||
|icon_image|text||
|user|references|null: false, foreign_key: true|
### Association
- belongs_to :user

## credit_cardsテーブル
|Column|Type|Options|
|------|----|-------|
|card_id|string|null: false|
|customer_id|string|null: false|
|user|references|null: false, foreign_key: true|
### Association
- belongs_to :user


## addressesテーブル
|Column|Type|Options|
|------|----|-------|
|firstname|string|null: false|
|lastname|string|null: false|
|firstname_kana|string|null: false|
|lastname_kana|string|null: false|
|postal_code|integer|null: false, limit: 8|
|prefectures|string|null: false|
|city|string|null: false|
|house_number|string|null: false|
|building_name|string||
|telephone_number|string||
|user|references|null: false, foreign_key: true|
### Association
- belongs_to :user

## sns_credentialsテーブル（SNS認証）
|Column|Type|Options|
|------|----|-------|
|provider|string|null: false| 
|uid|integer|null:false, unique: true|
|token|text||
|user|references|null: false, foreign_key: true|
### Association
- belongs_to :user

## itemsテーブル
|Column|Type|Options|
|------|----|-------|
|name|string|null: false|
|explanation|text|null: false|
|price|integer|null: false|
|category|references|null: false, foreign_key: true|
|brand|references|foreign_key: true|
|item_status|string|null: false|
|postage_type|string|null: false|
|postage_burden|string|null: false|
|shipping_area|string|null: false|
|shipping_date|string|null: false|
|user|references|null: false, foreign_key: true|
### Association
- belongs_to :user  
- has_many :comments, dependent: :destroy
- has_many :likes, dependent: :destroy
- has_many :images, dependent: :destroy
- belongs_to :category
- belongs_to :brand
- belongs_to_active_hash :item_condition  
- belongs_to_active_hash :postage_type  
- belongs_to_active_hash :postage_burden  
- belongs_to_active_hash :shipping_date

## categoriesテーブル
|Column|Type|Options|
|------|----|-------|
|name|string|null: false|
|ancestry|string||
### Association
- has_many: items

## brandsテーブル
|Column|Type|Options|
|------|----|-------|
|name|string|null: false|
### Association
- has_many :items

## imagesテーブル
|Column|Type|Options|
|------|----|-------|
|image|string|null: false|
|item|references|null: false, foreign_key: true|
### Association
- belongs_to :item

## likesテーブル
|Column|Type|Options|
|------|----|-------|
|user|references|null: false, foreign_key: true|
|item|references|null: false, foreign_key: true|
### Association
- belongs_to :user
- belongs_to :item

## commentsテーブル
|Column|Type|Options|
|------|----|-------|
|user|references|null: false, foreign_key: true|
|item|references|null: false, foreign_key: true|
|comment|text|null: false|
### Association
- belongs_to :user
- belongs_to :item


