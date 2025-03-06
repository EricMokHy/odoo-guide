# 文件編號自動生成 {#document-code-generation}
## Overview {#overview}
Odoo 會在創建報價單, 採購單, 發票時生成一個編號, 用以識別文件。然而這編碼不夠人性化, 使用者想達到的效果是能在這個編碼中, 大概了解該文件的相關訊息, 如客戶, 訂單種類, 建立日期等等。  

為此, 我們建立了[`文件編號規則`](#document-code-rules)。目前, 使用者需在建立報價單時手動生成這編號, 並手動輸入。這造成了許多人為錯誤, 也增加了使用者的不便, 尤其是在唯一性，引用這些情況下, 經常出現錯誤。

是次修改的目標就是要讓這些文件編號能自動生, 並自動引用, 減少人手輸入的煩鎖及錯誤的可能性。

## 文件編號規則 {#document-code-rules}
格式: `TTCCCBDDDDDDS`

`TT` 為文件種類

文件種類 | Type | 代碼
-- | -- | --
報價單 | Quotation | `QU`
客戶訂單 | Customer Order | `CO`
送貨單 | Delivery Note | `DN`
採購單 | Purchase Order | `PO`
發票 | Invoice | `IN`

`CCC` 為[客戶代碼](#customer-code)。  

`B` 是根據採購單所選的[產品種類](#product-type)計算。  

`DDDDDD` 為建立該報價單的日期, 格式為 `YYMMDD`。  

`S` 為每天的流水號, 該天的第一張報價單為`A`, 第二張為 `B`, 如些類推。以JS解釋為: `String.fromCharCode(64 + num)`。

### 示例 {#examples}

- 報價單: QUABCW240114A  
- 客戶訂單: COABCW240114A  
- 發票: INABCW240114A

### 文件編號的需求 {#document-code-requirements}
- 在報價單保存時自動生成, 不需要手動輸入  
- 使用者可以手動修改, 修改後的保存需檢查該編號的唯一性  
- 在建立報價單時, 同時生成 `CO`, `DN` 的編號  
- 在確認 `Quotation` 時, 會生成 `Customer Order` 並引用 `Quotation` 上的 `CO`編號  
- 在確認 `Quotation` 後, 文件編號 (`Code`) 不可修改。
- 在確認`Customer Order`時, 會生成`Invoice`, 並引用`Quotation`上的`Code`加上`IN`前綴作為該`Invoice`的編號。
- `Customer Order`, `Delivery Note`上的編號不可修改。

## 客戶代碼 {#customer-code}
客戶代碼 (Customer Code), 是保存在聯絡人(Contact)模組中的一個字段。這個字段有以下的特性。  

- 由三個大寫英文字母或數字組成  
- 當聯絡人為公司 (Company) 時, 這是必填的  
- 由使用者填寫  
- 客戶代碼對於該公司的所有客戶必須是唯一的  

> 設定 Odoo 的 Contact 表單  
> 1. 加入新字段, 命名為 `Customer Code`
> 2. Invisible: 當 `Company Type` = `Individual` 時不可見  
> 3. Required: 當 `Company Type` = `Company` 時為必填  
> 4. Label: `Customer Code`  
> 5. Help Tooltip: "3 digi, use for document code generation"  
> 6. Placeholder: "e.g. AAA"

## 產品種類 {#product-type}
產品種類 | Type | 編號
-- | -- | --
服務 | Service | `S`
貿易 | Consumable | `C`
庫存 | Storable | `W`
捆綁 | Bundle/Combo | `B`

> 當報價單裡的所有項目並非都為單一種類時, 或包含任何`Combo`種類的項目, 該報價單的類型都為 `B`

## 具體需求 {#specify-requirement}
### 聯絡人模組 {#contact-module}
#### 加入 Customer Code 字段 {#add-customer-code}
- 聯絡人加入 `customer_code` 字段, 長度為3  
- 當聯絡人的 `Company Type` 為 `Compnay` 時才顯示  
- 當聯絡人的`Company Type` 為 `Compnay` 時`company_code` 為必填項, 由使用者手動輸入  
- 在同一公司內, `customer_code` 必需是唯一的  

### 銷售模組 {#sale-module}
目前, 銷售模組已加入了 `Code`, `Quotation Code`, `Customer Order Code`, `Recipt Code`字段, 這些字段除了 `Code` 以外, 都為自動生成。

**字段的說明**  

字段名稱 | 用途 | 需求
-- | -- | --
`Code` | 原始的編號 (不包含文件類型的編號), 由使用者手動輸入 | 改為根據該採購單的內容自動生成
`Quotation Code` | 用於採購單本身的編號, 以 `Code` 加入 `QU` 前綴組成 |
`Customer Order Code` | 當確認採購單時自動生成 Customer Order, 這編號用於 Customer Order, 這編號由 `Code` 加上 `CO` 前綴組成 | 在生成 Customer Order 時把這字段帶到 Customer Order
`Recipt Code` | 使用者在生成了 Quotation 後可以隨時生成 Delivery Note, 這編號用於DN, 由 `Code` 加上 `DN` 前綴組成 |
`Invoice Code` | 並不在採購單中生成, 而是在 `Customer Order` 確認時, 會生成`Invoice`, 其時, 把`Code`帶到`Invoice`中, 並加上 `IN` 前綴組成。 |

#### 修改需求 {#modification-requirement-sale}
按目前需求, 只需要自動生成 `Code`。其規則如下:

- 在成功保存時觸發  
- 在`Confirm`前可手動修改
- 在`Confirm`時重新生成一次  
- 在`Confirm`後不可修改, 也不再重新生成  

`Code`的生成規則參考[文件編號規則](#document-code-rules)

### 採購模組 {#purchase-module}
- 在採購單加入報價單的`Quoation Code`的關聯  
- 使用者可以選擇性地關聯  
- 非必埴項  

### 會計模組 {#accounting-module}
- 在使用者確認`Customer Order`時會生成`Invoice`, 把`Quoation`裡的`Code`帶到Invoice裡, 並以此作為 `Invoice Code`。  
- `Invoice Code` 不可修改。

### 產品模組 {#product-module}  
由於要加入 Inventory, Purchase 模組讓系統更完整, 而現有的 Product 因未有針對這兩模組的優化, 在加入這兩個模組後會引致Product不合法而無法確認報價單的情況。  

這情況主要因為現時的Product都設為 `Consumable`, 在 Odoo 裡, Consumable 含有補貨的自動化邏輯, 在確認報價單時, 會同時生成相應的採購單。而生成採購單會關聯到採購路由(Route)。然而, 目前的 Product 都沒有這個Route的配置。  

在取得同事的意見後, 我們可以一次性的把目前的Product都統一改為`Service`, 這可避免在確認報價單時因Product缺了Route的錯誤。

#### 產品應有配置
##### Product Type
系統帶有4種產品類型

產品類型 | 說明
-- | --
Storable | 可儲存產品是您管理庫存的產品。
Consumable | 消耗品是沒有庫存管理的產品。常用於 Trading 的項目。
Service | 服務是你提供的非物質產品。
Combo | 是縱合兩種以上的。

> - 所以, 當該產品沒有實物時, 都應選`Service`。  
> - 在產品為實物時, 應同時設置 `Vendor`, `Route`。

##### Vendor
- Vendor 在 Odoo 裡是指供應商, 同一產品可以有多個供應商  
- 當產品為 `Storable` 或 `Consumable` 時, 應該加入最小一個供應商。  
- 每一個供應商可以配置不同的價格。  
- 供應商是來自於 Contact 的。

##### Inventory Route
- 在 Inventory 標籤裡, 當產品為`Storable` 或 `Consumable` 時, 應配置相應的`Route`

##### 產品名稱
目前系統中許多產品名稱以難以理解的編號作為標識，例如僅包含數字或無明確結構的代碼（如“123”或“ABC-001”）。這些命名方式存在以下問題：

- 難以辨識：產品名稱缺乏描述性，使用者無法僅憑名稱理解產品內容。  
- 無法重用：名稱沒有結構或規範，難以在未來的擴展或重用中保持一致。  
- 隨意性高：部分名稱過於隨意，未反映產品特性或用途，增加了搜索和管理的難度。  
  
為了解決這些問題，建議遵循以下原則撰寫產品名稱：

1. 描述性：名稱應包含關鍵資訊，例如產品類型、用途或特點。例如：  
        - 不良範例：123  
        - 優良範例：鋁合金窗框-銀色-90cm  
2. 一致性：名稱格式應統一，例如以產品類別-顏色/規格的形式呈現，方便搜索和排序。  
3. 避免冗長：名稱應簡潔明瞭，避免過多冗餘資訊，使名稱既清晰又實用。  
4. 使用標準縮寫：若需要縮短名稱，請使用常見且標準的縮寫，避免個人化的標記。例如，LG可代表“長”，WT可代表“白色”。  
5. 統一規範的產品名稱能提升系統的可讀性和管理效率，為後續的操作與維護奠定基礎。請全體使用者在新增產品時參考以上建議，確保名稱規範化與易用性。

