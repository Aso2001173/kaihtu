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
        c_namek
        postcode
        c_address
        email
        c_birthday
        c_regdate
    }
    
 entity "商品" as order <prodact> <<T,TRANSACTION_MARK_COLOR>> MAIN_ENTITY{
    +p_id[PK]
    --
     p_name
     gb_id
     p_model
     gc_name
     gs_id
     m_id
     p_saledate
     p_price
    
    total_price
    }
entity "購入詳細テーブル" as order_detail <d_purchase> <<T,TRANSACTION_MARK_COLOR>> MAIN_ENTITY{
    +order_id[PK]
    +detail_id[PK]
    --
    #item_code[FK]
   price
   num
   }
    entity "商品マスタ" as items <m_items> <<M,MASTER_MARK_COLOR>> {
        + item_code [PK]
        --
        item_name
        price
        # category_id
        image
        detail
        del_flag
        reg_date
    }
 entity "カテゴリマスタ" as category <m_category> <<M,MASTER_MARK_COLOR>> {
        + category_id [PK]
        --
        name
        reg_date
    }   
  }
 customer       |o-ri-o{     order
order          ||-ri-|{     order_detail
order_detail    }-do-||     items
items          }o-le-||     category 
@enduml
```