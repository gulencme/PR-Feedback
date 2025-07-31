## Pull Request Feedback Summary

### XPAY

#### 🔹 **Req1: X-Api-Key Zorunluluğu**

- **Yorum:**

```
Tung Beier:
  there's no need to specify this as it is only used by Kong to route the requests to the corresponding backend

pfaefflin-mms:
  It will be routed via Kong, see url comment.
```

- **Durum:** **Çözüldü**

- **Açıklama:** Kong tarafından yönlendirme için kullanıldığından X-Api-Key gereksinimi devam ediyor. Kaldırılmayacak.

---

#### 🔹 **Req2: LineItemId Format Değişikliği**

- ** Yorum yapılan kod**
```
BasketItem:
  required:
    - line_item_id
    - quantity
    - unit_price
  type: object
  properties:
    line_item_id:
      type: integer
      description: Unique ID of the basket line item
      format: int64
```

- **Yorum:**

```
(28.07.2025 - 16:20) Tung Beier:
  maybe consider using int32 instead. int64 is quite big in my opinion for items in a basket.
(30.07.2025 - 09:03) Florian Heubeck:
  and if it's some kind of technical identifier, please use string
(30.07.2025 - 12:20) gulencme:
  This field represents the sequential line number of items within the basket (1, 2, 3...) and must be unique per basket. Using int32 is appropriate since this is not an identifier, but simply a sequential number. I'll update the description to make it clearer.
```

- **Durum:** **Çözüldü**

- **Açıklama:** LineItemId formatı int64'ten int32'ye çevrildi. Description iyileştirildi.

---

#### 🔹 **Req3: Quantity Minimum Değeri**

- ** Yorum yapılan kod**
```
quantity:
  minimum: 0
  exclusiveMinimum: true
  type: integer
  description: Number of units purchased
  format: int32
```

- **Yorum:**

```
(28.07.2025 - 16:25) Tung Beier:
  quantity:
    minimum: 1
    exclusiveMinimum: false

  or just

  quantity:
    minimum: 1

  because exclusiveMinimum default is false, would be easier to understand in my opinion.
```

- **Durum:** **Çözüldü**

- **Açıklama:** exclusiveMinimum kaldırılarak sadece minimum: 1 kullanıldı.

---

#### 🔹 **Req4: Unit Price Money Object Kullanımı**

- ** Yorum yapılan kod**
```
unit_price:
  minimum: 0
  exclusiveMinimum: true
  type: number
  description: Price per single item
  format: decimal
```

- **Yorum:**

```
(28.07.2025 - 16:26) Tung Beier:
  shouldn't this be declared as a money object?
  https://github.com/MediaMarkt/Saturn/platform/api-design-guide/blob/master/MediaMarkt-00-money
  and other price properties as well?
```

- **Durum:** **Çözüldü**

- **Açıklama:** Unit price ve diğer fiyat alanlarının money object olarak tanımlandı.

---

#### 🔹 **Req5: CreateRefundRequestModel'de Model Suffix'inin Kaldırılması**

- ** Yorum yapılan kod**
```
CreateRefundRequestModel:
      required:
        - refund_request_id
      type: object
```

- **Yorum:**

```
(28.07.2025 - 16:20) Tung Beier:
  in my opinion (and this is purely my opinion and not some guideline), there's no need to use a suffix in this case, ModelL, because it doesn't add any useful meaning to the object. And any openapi generators that I know offer an option to add a pre- and/or suffix of your choice to the generated models if you really want to have them in your code.
  Also this adds inconsistency to all the components in this spec as well, because the suffix is not use in all components.

(29.07.2025 - 15:52) gulencme:
  At the code level, we use suffixes in all our components for differentiation purposes as an architectural approach. We prefer to keep it this way if it won't affect the current structure, as it would require extensive changes

(29.07.2025 - 15:54) Florian Heubeck:
  And any openapi generators that I know offer an option to add a pre- and/or suffix of your choice to the generated models if you really want to have them in your code.

```

- **Durum:** **Bekliyor**

- **Açıklama:** Model isimlerindeki "Model" suffix'inin kaldırılması konusunda Mehmet abi ile tartışılıp, karar verilecek.

---

#### 🔹 **Req6: Customer ID Sistem Açıklaması**

- ** Yorum yapılan kod**
```
CustomerInfo:
  required:
    - customer_id
    - email
    - name
    - phone
  type: object
  properties:
    email:
      minLength: 1
      type: string
      description: Customer email address
    customer_id:
      minLength: 1
      type: string
      description: Customer identifier
    name:
      minLength: 1
      type: string
      description: Full name of the customer
    phone:
      minLength: 1
      type: string
      description: Phone number of the customer
  additionalProperties: false
```

- **Yorum:**

```
(28.07.2025 - 16:39) Tung Beier:
  EN: customer's id in which system?
  TR: Müşterinin kimliği hangi sistemde?
(30.07.2025 - 09:10) Florian Heubeck:
  EN: if's that what I think it should be, please call it party_id for clarity.
  TR: eğer düşündüğüm buysa, açıklık sağlamak için lütfen buna party_id diyelim.
```

- **Durum:** **Bekliyor**

- **Açıklama:** Customer ID'nin hangi sistemde olduğuna dair Ömer abi'den cevap bekleniyor.

---

#### 🔹 **Req7: Postal Code Zorunluluğu**

- ** Yorum yapılan kod**
```
DeliveryAddress:
  required:
    - city
    - contact_name
    - country
    - street
```

- **Yorum:**

```
(28.07.2025 - 16:39) Tung Beier:
  shouldn't postal_code be required as well?
```

- **Durum:** **Çözüldü**

- **Açıklama:** Postal code alanı zorunlu hale getirildi.

---

#### 🔹 **Req8: Error Data Type Belirlenmesi**

- ** Yorum yapılan kod**
```
ErrorDetailModel:
    required:
      - code
    type: object
    properties:
      code:
        type: string
        description: Error detail code
      message:
        type: string
        description: Error detail message
        nullable: true
      data:
        description: Error detail data
        nullable: true
    additionalProperties: false
```
      
- **Yorum:**

```
(28.07.2025 - 16:39) Tung Beier:
  what type is this data? an object, a string, a number?
(30.07.2025 - 09:11) Florian Heubeck:
  why not problem+json again?
(30.07.2025 - 15:25) Mehmet Gülenç
  Could you please add a bit more detail here? Just as a reminder, the contentType (e.g. application/problem+json) should live in the response’s content section, not inside a schema object. For example, in the current file we already use it like this:
  default:
    description: An error occurred. See payload for error code, message and optional error details.
    content:
      application/problem+json:
        schema:
          $ref: '#/components/schemas/ErrorResponseModel'
```

- **Durum:** **Bekliyor**

- **Açıklama:** Error modelindeki data alanını generic object'den belli bir obje olarak dönüştürülecek. Yapı değişikliği olduğundan kaynaklı biraz uzun sürebilir.

---

#### 🔹 **Req9: FulfillmentMethod Enum Değerleri**

- **Yorum:**

```
(28.07.2025 - 16:42) Tung Beier:
  why not write this out like the other values as well?

  Suggested change:
  - SDD_EXPRESS --> SAME_DAY_DELIVERY_EXPRESS

(28.07.2025 - 19:16) pfaefflin-mms:
  This value is already defined this way in XPAY and other systems
```

- **Durum:** **Bekliyor**

- **Açıklama:** FulfillmentMethod enum değerlerinin tam olarak yazılması Thomas Pfaefflin'a soruldu. Cevap bekleniyor.
- **Cevap olarak:** SDD_EXPRESS kaldırılması ve sadece SAME_DAY_DELIVERY şeklinde devam edilmesi istendi.

---

#### 🔹 **Req10: Detaylı Proje Açıklaması**

- **Yorum:**

```
(28.07.2025 - 16:09) Tung Beier:
  EN: please provide a description with more substance. This doesn't help anyone to understand what this spec is used for.
  TR: Lütfen daha kapsamlı bir açıklama sağlayın. Bu açıklama kimsenin bu spesifikasyonun ne için kullanıldığını anlamasına yardımcı olmuyor.
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'den daha detaylı açıklama alınacak.

---

#### 🔹 **Req11: Versiyonlamanın server url levelinde yazılması**

- ** Yorum yapılan kod**
```
paths:
  /v1/refunds:
```

- **Yorum:**

```
(30.07.2025 - 09:01) Florian Heubeck:
  EN: isn't it api-gateway/v1/api-name/your-resources usually? so v1 to be part of the server url. (same for all other paths)
  TR: Genellikle api-gateway/v1/api-name/your-resources şeklinde olmaz mı? Yani v1'in server URL'inin bir parçası olması gerekir. (Diğer tüm path'ler için de aynı şekilde)

İstenen çözüm:
  paths:
    /refunds:
```

- **Etkileri:** Endpoint bazlı versiyonlama özelliği uygulamalarda kaldırılmış olacak.

- **Durum:** **Bekliyor**

- **Açıklama:** İncelendi. Ömer abi'den son yorum alınacak.

---

#### 🔹 **Req12: BasketItems altında bulunan product_id açıklaması talebi ?**

- ** Yorum yapılan kod**
```
product_id:
  type: string
  description: ID of the product
```

- **Yorum:**
```
(30.07.2025 - 09:03) Florian Heubeck:
  EN: MDNG id? global product id?
  TR: "MDNG id? global ürün id'si?"
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'ye sorulacak.

---

#### 🔹 **Req13: CreateRefundRequestModel altında bulunan payment_id parametresi identifier olarak değiştirilmeli**

- ** Yorum yapılan kod**
```
payment_id:
  type: integer
  description: Payment ID for refunding order fully or partially
  format: int64
  nullable: true
```

- **Yorum:**
```
(30.07.2025 - 09:06) Florian Heubeck:
  https://github.com/MediaMarktSaturn/oas/tree/master/api-design-guide#oas-09-business-object-identifiers
```

- **Durum:** **Bekliyor**

- **Açıklama:** Gerekliliği Ömer abi'ye sorulacak.

---

#### 🔹 **Req14: Yabancı para birimi olacak mı ?**

- ** Yorum yapılan kod**
```
Currency:
  pattern: ^[A-Z]{3}$
  type: string
  description: Represents supported currencies.
  format: string
  example: TRY
  x-extensible-enum:
    - TRY
```

- **Yorum:**
```
(30.07.2025 - 09:10) Florian Heubeck:
  no foreign currency payments?
```

- **Durum:** **Bekliyor**

- **Açıklama:** Hayır şimdilik olmayacak ama uzun vadede olup olmayacağı Ömer abi'ye sorulacak.

---

#### 🔹 **Req15: BasketItems içinde bulunan ItemType adlandırma değişikliği talebi**

- ** Yorum yapılan kod**
```
ItemType:
  type: string
  x-extensible-enum:
    - PHYSICAL
    - DIGITAL
    - WARRANTY
    - SERVICE
```

- **Yorum:**
```
(30.07.2025 - 09:16) Florian Heubeck:
  EN: who's deciding for those? would prefer to have kind_of_product.
  TR: Bunlara kim karar veriyor? kind_of_product olmasını tercih ederdim.
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'ye sorulacak.

---

#### 🔹 **Req16: RefundResultModel altında status enum olmalı**

- ** Yorum yapılan kod**
```
RefundResultModel:
  required:
    - status
  type: object
  properties:
    status:
      minLength: 1
      type: string
      description: Refund status. e.g., pending, success, failed
  additionalProperties: false
```

- **Yorum:**
```
(30.07.2025 - 09:17) Florian Heubeck:
  enumeration?
```

- **Durum:** **Çöüzldü**

- **Açıklama:** Enum'a çevrildi

---

#### 🔹 **Req17: SellerType enum verisi değişikliği**

- ** Yorum yapılan kod**
```
SellerType:
  type: string
  x-extensible-enum:
    - RETAIL
    - MARKET_PLACE
    - MIX_BASKET
```

- **Yorum:**
```
(30.07.2025 - 09:17) Florian Heubeck:
  MIX_BASKET -> MIXED_BASKET
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'ye sorulacak.

---
