## Gateway Payment Api Project Pull Request Feedback Summary

**📅 Son Güncelleme:** 01 Ağustos 2025 - 11:48

## 📊 Durum Özeti

| **Durum** | **Sayı** |
|-----------|----------|
| ✅ **Çözüldü** | 10 |
| ⏳ **Bekliyor** | 8 |
| **📝 Toplam** | **18** |

---

### Çözülen Konular (10):
- Req1: X-Api-Key Zorunluluğu
- Req2: LineItemId Format Değişikliği  
- Req3: Quantity Minimum Değeri
- Req4: Unit Price Money Object Kullanımı
- Req6: Customer ID Sistem Açıklaması
- Req7: Postal Code Zorunluluğu
- Req9: FulfillmentMethod Enum Değerleri
- Req12: Product ID Açıklaması
- Req15: Yabancı Para Birimi
- Req17: RefundResultModel altında status enum
- Req18: SellerType Enum Değişikliği

### Bekleyen Konular (8):
- Req5: Model Suffix Kaldırılması
- Req8: Error Data Type Belirlenmesi
- Req10: Detaylı Proje Açıklaması
- Req11: Versiyonlama Server URL'de
- Req13: Product Group ID Tip Değişikliği
- Req14: Payment ID String Yapılması
- Req16: ItemType Adlandırma Değişikliği
  
---

#### 🔹 **Req1: X-Api-Key Zorunluluğu**

```
parameters:
  - name: Accept-Language
    in: header
    schema:
      type: string
  - name: X-Flow-Id
    in: header
    required: true
    schema:
      type: string
  - name: X-Api-Key
    in: header
    schema:
      type: string
```

- **Yorum:**

```
(28.07.2025 - 16:13) Tung Beier:
  there's no need to specify this as it is only used by Kong to route the requests to the corresponding backend

(28.07.2025 - 19:14) pfaefflin-mms:
  It will be routed via Kong, see url comment.

(29.07.2025 - 16:02) Tung Beier:
  what I mean is that it can be ommitted in the spec, because a lot of specs are doing it.
  but of course we can have it in here for clarity.
```

- **Durum:** **Çözüldü**

- **Açıklama:** Kong tarafından yönlendirme için kullanıldığından X-Api-Key gereksinimi devam ediyor. Kaldırılmayacak.

---

#### 🔹 **Req2: LineItemId Format Değişikliği**

- **Yorum yapılan kod:**
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
EN:
(28.07.2025 - 16:20) Tung Beier:
  maybe consider using int32 instead. int64 is quite big in my opinion for items in a basket.
(30.07.2025 - 09:03) Florian Heubeck:
  and if it's some kind of technical identifier, please use string
(30.07.2025 - 12:20) Mehmet Gülenç:
  This field represents the sequential line number of items within the basket (1, 2, 3...) and must be unique per basket. Using int32 is appropriate since this is not an identifier, but simply a sequential number. I'll update the description to make it clearer.
```
```
TR:
(28.07.2025 - 16:20) Tung Beier:
belki int32 kullanmayı düşünebilirsin. int64 sepetteki öğeler için bence oldukça büyük.
(30.07.2025 - 09:03) Florian Heubeck:
ve eğer bu bir tür teknik tanımlayıcı ise, lütfen string kullan
(30.07.2025 - 12:20) Mehmet Gülenç:
Bu alan sepet içindeki öğelerin sıralı satır numarasını temsil ediyor (1, 2, 3...) ve sepet başına benzersiz olmalı. Bu bir tanımlayıcı değil, sadece sıralı bir numara olduğu için int32 kullanmak uygun. Açıklamayı daha net hale getirmek için güncelleyeceğim.
```

- **Durum:** **Çözüldü**

- **Açıklama:** LineItemId formatı int64'ten int32'ye çevrildi. Description iyileştirildi.

---

#### 🔹 **Req3: Quantity Minimum Değeri**

- **Yorum yapılan kod:**
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

- **Yorum yapılan kod:**
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

- **Yorum yapılan kod:**
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

(29.07.2025 - 15:52) Mehmet Gülenç:
  At the code level, we use suffixes in all our components for differentiation purposes as an architectural approach. We prefer to keep it this way if it won't affect the current structure, as it would require extensive changes

(29.07.2025 - 15:54) Florian Heubeck:
  And any openapi generators that I know offer an option to add a pre- and/or suffix of your choice to the generated models if you really want to have them in your code.

```

- **Durum:** **Bekliyor**

- **Açıklama:**
  - (29.07.2025) Model isimlerindeki "Model" suffix'inin kaldırılması konusunda Mehmet abi ile tartışılıp, karar verilecek.
  - (31.07.2025) Ömer abi ile konuşuldu. PR linki verildi. Direkt kendisinin yorum yapması bekleniyor.

---

#### 🔹 **Req6: Customer ID Sistem Açıklaması**

- **Yorum yapılan kod:**
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
(01.08.2025 - 07:51) Mehmet Gülenç:
  EN: It has been handled as it is used and named in the customer order service v3 (COS V3) systems. This value is the customer's identity ID in COS. It is not the party_id value.
  TR: customer order service v3 (COS V3) sistemlerinde kullanıldığı ve adlandırıldığı gibi ele alınmıştır. Bu değer COS'da müşterinin kimlik id'sidir. Party_id değeri değildir.
(01.08.2025 - 07:54) Tung Beier:
  EN: please add this info to its description then
  TR: lütfen bu bilgiyi açıklamasına ekleyin
(01.08.2025 - 08:02) Mehmet Gülenç:
  EN: Yes, you're right. The descriptions are currently insufficient. We have started working to review and update all descriptions.
  TR: Evet haklısınız. Şu an açıklamalar yetersiz. Tüm descriptionları gözden geçirip güncellemek için çalışma başlattık.
```

- **Durum:** **Çözüldü**

- **Açıklama:**
  - (30.07.2025) Customer ID'nin hangi sistemde olduğuna dair Ömer abi'den cevap bekleniyor.
  - (01.08.2025) Açıklama yapıldı.

---

#### 🔹 **Req7: Postal Code Zorunluluğu**

- **Yorum yapılan kod:**
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

- **Yorum yapılan kod:**
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
(31.07.2025 - 15:27) Tung Beier:
  I mean the type is missing
  data:
  + type: ...
    description: Error detail data
    nullable: true
(31.07.2025 - 15:33) Mehmet Gülenç
  This is a generic field - if I set the data type to object, will you expect a specific $ref?
The data field should remain generic to maintain flexibility for different error contexts while providing proper type definition.
```

- **Durum:** **Bekliyor**

- **Açıklama:** Tung Beier ile olan konuşma devam etmekte. İstek net olduğunda düzenlenecektir.

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

- **Durum:** **Çözüldü**

- **Açıklama:**
  - FulfillmentMethod enum değerlerinin tam olarak yazılması Thomas Pfaefflin'a soruldu. Cevap bekleniyor.
  - Thomas Pfaefflin "SDD_EXPRESS" kaldırılması ve sadece "SAME_DAY_DELIVERY" şeklinde devam edilmesi istendi.
---

#### 🔹 **Req10: Detaylı Proje Açıklaması**

- **Yorum:**

```
(28.07.2025 - 16:09) Tung Beier:
  EN: please provide a description with more substance. This doesn't help anyone to understand what this spec is used for.
  TR: Lütfen daha kapsamlı bir açıklama sağlayın. Bu açıklama kimsenin bu spesifikasyonun ne için kullanıldığını anlamasına yardımcı olmuyor.
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'ye iki projenin spec'i gönderilip tüm parametreler için detaylı açıklama girmesi istenecek.

---

#### 🔹 **Req11: Versiyonlamanın server url levelinde yazılması**

- **Yorum yapılan kod:**
```
paths:
  /v1/refunds:
```

- **Yorum:**

```
(30.07.2025 - 09:01) Florian Heubeck:
  EN: isn't it api-gateway/v1/api-name/your-resources usually? so v1 to be part of the server url. (same for all other paths)
  TR: Genellikle api-gateway/v1/api-name/your-resources şeklinde olmaz mı? Yani v1'in server URL'inin bir parçası olması gerekir. (Diğer tüm path'ler için de aynı şekilde)

Suggested change:
  paths:
    /refunds:

(01.08.2025 - 08:45) Mehmet Gülenç:
  Just to confirm - if we move the version to the server URL as suggested, we would lose our endpoint-level versioning flexibility. Is this the intended approach you'd prefer?
```

- **Etkileri:** Endpoint bazlı versiyonlama özelliği uygulamalarda kaldırılmış olacak.

- **Durum:** **Bekliyor**

- **Açıklama:**
  - (30.07.2025) İncelendi. Ömer abi'den son yorum alınacak.
  - (01.08.2025) Florian Heubeck'den onay bekleniyor.

---

#### 🔹 **Req12: BasketItems altında bulunan product_id açıklaması talebi ?**

- **Yorum yapılan kod:**
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
(01.08.2025 - 08:22) Mehmet Gülenç:
  It is the (MDNG) ID
```

- **Durum:** **Çözüldü**

- **Açıklama:**
  - (30.07.2025) Ömer abi'ye sorulacak.
  - (01.08.2025) Açıklama yapıldı.

---

#### 🔹 **Req13: BasketItems altında bulunan product_group_id tip değişikliği talebi**

- **Yorum yapılan kod:**
```
product_group_id:
  minimum: 0
  exclusiveMinimum: true
  type: integer
  description: Product category or group ID
  format: int32
  nullable: true
```

- **Yorum:**
```
(30.07.2025 - 09:04) Florian Heubeck:
  EN: product group id is an identifier, please use string
  TR: ürün grubu kimliği bir tanımlayıcıdır, lütfen dize kullanın
(01.08.2025 - 08:23) Mehmet Gülenç:
  EN: The "https://api.mediamarktsaturn.com/v2/sales-products" service accepts the product_group_id parameter as int. We use this service to retrieve the product_group_id values of products. We manage the installment options we offer to our customers based on the product_group_id information. The sales-products v2 service sends int for product_group_id. To maintain consistency, this value should also be int in the service we provide to you
  TR: "https://api.mediamarktsaturn.com/v2/sales-products" servisi product_group_id parametresini int olarak alıyor. Bu servisi kullanarak ürünlerin product_group_id'lerini alıyoruz. product_group_id bilgisi ile müşterilerimize sunacağımız taksit seçeneklerini yönetiyoruz. sales-products v2 servisi product_group_id için int gönderiyor. Bütünlüğünü sağlamak için bu değerin size verdiğimiz serviste de int olması gerekmektedir.
```

- **Durum:** **Bekliyor**

- **Açıklama:**
  - (30.07.2025) Ömer abi'ye sorulacak.
  - (01.08.2025) Açıklama yapıldı. Son yorum Florian Heubeck'den bekleniyor.

---

#### 🔹 **Req14: CreateRefundRequestModel altında bulunan payment_id parametresi identifier olarak değiştirilmeli**

- **Yorum yapılan kod:**
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
(01.08.2025 - 08:29) Mehmet Gülenç:
  EN: We don't generate this value. Since the integrator's own value is of type int, we need to maintain the "payment_id" parameter as int type on our side as well to ensure consistency
  TR: Bu değeri biz oluşturmuyoruz. Entegratörün kendi değeri int tipinde olduğundan dolayı, tutarlılığı sağlamak için "payment_id" parametresinin tipini bizim tarafımızda da int olarak tutmamız gerekmektedir.
(30.07.2025 - 10:00) Florian Heubeck:
  EN: that doesn't necessary stay this way. so even more important for identifiers out of your control, this rule of the guide is valid.
  TR: bunun bu şekilde kalması gerekmiyor. bu yüzden kontrolünüz dışındaki tanımlayıcılar için daha da önemli, kılavuzun bu kuralı geçerli.
```

- **Durum:** **Bekliyor**

- **Açıklama:**
  - (30.07.2025) Gerekliliği Ömer abi'ye sorulacak.
  - (01.08.2025 - 08:30) Soruldu ve yorum yapıldı.
  - (01.08.2025 - 10:54) Tekrardan açıldı, Ömer abiye iletildi, yorumu bekleniyor.

---

#### 🔹 **Req15: Yabancı para birimi olacak mı ?**

- **Yorum yapılan kod:**
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
(01.08.2025 - 08:01) Mehmet Gülenç:
  No, only support TRY.
```

- **Durum:** **Çözüldü**

- **Açıklama:**
  - (31.07.2025) Hayır şimdilik olmayacak ama uzun vadede olup olmayacağı Ömer abi'ye sorulacak.
  - (01.07.2025) Soruldu. Uzun vade de olmayacak.

---

#### 🔹 **Req16: BasketItems içinde bulunan ItemType adlandırma değişikliği talebi**

- **Yorum yapılan kod:**
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
(01.08.2025 - 07:57) Mehmet Gülenç:
  EN: We decided on ItemType because it represents the product type for us. Unfortunately, we cannot change it to kind_of_product. Because kind_of_product has a different meaning in COS V3 and this project has a different reporting branch, so it would create confusion with different meanings.
In COS V3, it is as follows:
kind_of_product:
  type: string
  description: An MMS internal classification of product, provided by ProductAPI.
  example: 'MEAT_GRINDER'
  TR: ItemType kullanmaya karar verdik çünkü bizim için ürün tipini temsil ediyor. Maalesef kind_of_product olarak değiştiremeyiz. Çünkü kind_of_product COS V3'te farklı bir anlama sahip ve bu projenin farklı bir raporlama dalı var, bu nedenle farklı anlamlarla karışıklık yaratacaktır.
COS V3'te şu şekildedir:
kind_of_product:
  type: string
  description: An MMS internal classification of product, provided by ProductAPI.
  example: 'MEAT_GRINDER'

```

- **Durum:** **Bekliyor**

- **Açıklama:**
  - (31.07.2025) Ömer abi'ye sorulacak.
  - (01.08.2025) Soruldu ve yorum yapıldı. Florian Heubeck'den dönüş bekleniyor.

---

#### 🔹 **Req17: RefundResultModel altında status enum olmalı**

- **Yorum yapılan kod:**
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
(01.08.2025 - 07:53) Mehmet Gülenç:
  We will organize it as an enum
```

- **Durum:** **Çöüzldü**

- **Açıklama:** Enum'a çevrildi

---

#### 🔹 **Req18: SellerType enum verisi değişikliği**

- **Yorum yapılan kod:**
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
(01.08.2025 - 07:59) Mehmet Gülenç:
  The MIX_BASKET value will be changed to MIXED_BASKET.
```

- **Durum:** **Çözüldü**

- **Açıklama:**
  - (31.07.2025) Ömer abi'ye sorulacak.
  - (01.08.2025) Soruldu, MIXED_BASKET olarak değiştirilecek.

---
