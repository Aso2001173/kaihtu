```startuml
@startuml

/'
  図の中で目立たせたいエンティティに着色するための
  色の名前（定数）を定義できます。
  ↓色のサンプル↓
  https://github.com/shibakay/2021sys-design/blob/main/color.md
'/

!define MASTER_MARK_COLOR Orange 
!define TRANSACTION_MARK_COLOR DeepSkyBlue

'グラデーションさせる場合 #xx-xx
!define MAIN_ENTITY #MintCream-MistyRose

/'
  デフォルト色を"skinparam class"で設定します。
'/
skinparam class {
    '図の背景
    BackgroundColor Snow
    '図の枠
    BorderColor Black
    'リレーションの色
    ArrowColor Black
}

package "ECサイト" as target_system {
    /'
      マスターテーブルを M、トランザクションを T などで表記
      １文字なら "主" とか "従" まど日本語でも記載可能
     '/

    entity "会員" as customer <customers> <<M,MASTER_MARK_COLOR>> {
        +  c_id[PK]
        --
        c_name
        c_frigana
        c_postcode
        c_address
        c_email
        c_regdate
    }
    
 entity "商品" as prodact <prodact> <<T,TRANSACTION_MARK_COLOR>> MAIN_ENTITY{
    +p_id[PK]
    --
     p_name
     p_model
     g_id[FK]
     m_name
     p_saledate
     p_price
     p_pr
     p_photo_path
    }

    entity "ジャンル" as genre <genre> <<M,MASTER_MARK_COLOR>> {
        +g_id [PK]
        --
        g_name
    }
 entity "パスワード" as password <password> <<M,MASTER_MARK_COLOR>> {
        +pw_id [PK]
        --
        pw_value
    } 
 entity "カート" as  cart <cart> <<M,MASTER_MARK_COLOR>> {
        + c_id[PK]
        --
        p_id[FK]
        cart_qty
       }
 entity "注文詳細" as order_details <order_details> <<M,MASTER_MARK_COLOR>> {    
      +od_id[PK]
      +c_id[PK]
      --
      p_id[PK]
      order_qty
      unitprice
      }
 entity "レビュー" as review <review> <<M,MASTER_MARK_COLOR>> { 
      +p_id[PK]
      --
      c_id[FK]
      r_coment
      r_sutars
 } 
entity "注文" as order <order> <<M,MASTER_MARK_COLOR>> { 
    +o_id[PK]
    --
    o_date[FK]
    c_id
    tax
    delivery_cost
    sumprice
    delivery_day
    delivery_address
    delivery_name
}
customer ||--||cart
cart ----o{ prodact
prodact }o--|{ genre
customer ||--|| password
customer ||--|{ order_details
customer ||--o{ review
prodact ||--o{ review
customer ||----o{ order
@enduml
```
