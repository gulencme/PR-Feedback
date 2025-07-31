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

- **Yorum:**

```
Tung Beier:
  maybe consider using int32 instead. int64 is quite big in my opinion for items in a basket.
```

- **Durum:** **Çözüldü**

- **Açıklama:** LineItemId formatı int64'ten int32'ye değiştirildi.

---

#### 🔹 **Req3: Quantity Minimum Değeri**

- **Yorum:**

```
Tung Beier:
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

- **Yorum:**

```
Tung Beier:
  shouldn't this be declared as a money object?
  https://github.com/MediaMarkt/Saturn/platform/api-design-guide/blob/master/MediaMarkt-00-money
  and other price properties as well?
```

- **Durum:** **Çözüldü**

- **Açıklama:** Unit price ve diğer fiyat alanlarının money object olarak tanımlandı.

---

#### 🔹 **Req5: CreateRefundRequestModel Suffix Kaldırılması**

- **Yorum:**

```
Tung Beier:
  in my opinion (and this is purely my opinion and not some guideline), there's no need to use a suffix in this case, ModelL, because it doesn't add any useful meaning to the object. And any openapi generators that I know offer an option to add a pre- and/or suffix of your choice to the generated models if you really want to have them in your code.
  Also this adds inconsistency to all the components in this spec as well, because the suffix is not use in all components.
```

- **Durum:** **Bekliyor**

- **Açıklama:** Model isimlerindeki "Model" suffix'inin kaldırılması konusunda tartışılıp, karar verilecek.

---

#### 🔹 **Req6: Customer ID Sistem Açıklaması**

- **Yorum:**

```
Tung Beier:
  EN: customer's id in which system?
  TR: Müşterinin kimliği hangi sistemde?
Florian Heubeck 
  EN: if's that what I think it should be, please call it party_id for clarity.
  TR: eğer düşündüğüm buysa, açıklık sağlamak için lütfen buna party_id diyelim.
```

- **Durum:** **Bekliyor**

- **Açıklama:** Customer ID'nin hangi sistemde olduğuna dair açıklama eklenmesi bekleniyor.

---

#### 🔹 **Req7: Postal Code Zorunluluğu**

- **Yorum:**

```
Tung Beier:
  shouldn't postal_code be required as well?
```

- **Durum:** **Çözüldü**

- **Açıklama:** Postal code alanı zorunlu hale getirildi.

---

#### 🔹 **Req8: Error Data Type Belirlenmesi**

- **Yorum:**

```
Tung Beier:
  what type is this data? an object, a string, a number?
  
```

- **Durum:** **Bekliyor**

- **Açıklama:** Error modelindeki data alanını generic object'den belli bir obje olarak dönüştürülecek.

---

#### 🔹 **Req9: FulfillmentMethod Enum Değerleri**

- **Yorum:**

```
Tung Beier:
  why not write this out like the other values as well?

  Suggested change:
  - SDD_EXPRESS --> SAME_DAY_DELIVERY_EXPRESS

pfaefflin-mms:
  This value is already defined this way in XPAY and other systems
```

- **Durum:** **Bekliyor**

- **Açıklama:** FulfillmentMethod enum değerlerinin tam olarak yazılması Thomas Pfaefflin'a soruldu. Cevap bekleniyor.
- Cevap olarak : SDD_EXPRESS kaldırılması ve sadece SAME_DAY_DELIVERY şeklinde devam edilmesi istendi.

#### 🔹 **Req10: Detaylı Proje Açıklaması**

- **Yorum:**

```
Tung Beier:
  EN: please provide a description with more substance. This doesn't help anyone to understand what this spec is used for.
  TR: Lütfen daha kapsamlı bir açıklama sağlayın. Bu açıklama kimsenin bu spesifikasyonun ne için kullanıldığını anlamasına yardımcı olmuyor.
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'den daha detaylı açıklama alınacak.

#### 🔹 **Req11: Versiyonlamanın server url levelinde yazılması**

- **Yorum:**

```
Florian Heubeck:
  EN: isn't it api-gateway/v1/api-name/your-resources usually? so v1 to be part of the server url. (same for all other paths)
  TR: Genellikle api-gateway/v1/api-name/your-resources şeklinde olmaz mı? Yani v1'in server URL'inin bir parçası olması gerekir. (Diğer tüm path'ler için de aynı şekilde)
```

- **Durum:** **Bekliyor**

- **Açıklama:** İncelendi. Çözümü için endpoint bazlı versiyonlama özelliği kaldırılmış olacak. Ömer abi'den son yorum alınacak.

#### 🔹 **Req12: BasketItems altında bulunan ProductId nedir ?**

- ** Örnek Kod""
```
product_id:
  type: string
  description: ID of the product
```

- **Yorum:**
```
Florian Heubeck:
  EN: MDNG id? global product id?
  TR: "MDNG id? global ürün id'si?"
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'ye sorulacak.

#### 🔹 **Req13: CreateRefundRequestModel altında bulunan payment_id parametresi identifier olarak değiştirilmeli**

- ** Örnek Kod""
```
payment_id:
  type: integer
  description: Payment ID for refunding order fully or partially
  format: int64
  nullable: true
```

- **Yorum:**
```
Florian Heubeck:
  https://github.com/MediaMarktSaturn/oas/tree/master/api-design-guide#oas-09-business-object-identifiers
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'ye sorulacak.

#### 🔹 **Req14: Yabancı para birimi olacak mı ?**

- **Yorum:**
```
Florian Heubeck:
  no foreign currency payments?
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'ye sorulacak.

#### 🔹 **Req15: BasketItems içinde bulunan ItemType adlandırma değişikliği talebi**

- **Yorum:**
```
Florian Heubeck:
  EN: who's deciding for those? would prefer to have kind_of_product.
  TR: Bunlara kim karar veriyor? kind_of_product olmasını tercih ederdim.
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'ye sorulacak.

#### 🔹 **Req16: RefundResultModel altında status enum olmalı**

- **Yorum:**
```
Florian Heubeck:
  enumeration?
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'ye sorulacak.

#### 🔹 **Req17: SellerType enum verisi değişikliği**

- **Yorum:**
```
Florian Heubeck:
  MIX_BASKET -> MIXED_BASKET
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'ye sorulacak.
