## Provider Project Pull Request Feedback Summary

**📅 Son Güncelleme:** 31 Temmuz 2025 - 14:52

## 📊 Durum Özeti

| **Durum** | **Sayı** |
|-----------|----------|
| ✅ **Çözüldü** | 20 |
| ⏳ **Bekliyor** | 5 |
| **📝 Toplam** | **25** |

---

### Çözülen Konular (20):
- Req2: Kullanılmayan Parametrelerin Kaldırılması
- Req4: Gereksiz Description Tekrarlarının Kaldırılması
- Req5: Kod Biçimlendirme ve Gereksiz allOf Kullanımı
- Req6: allOf Kaldırılması (Tüm modellerde)
- Req7: Date-Time Örneklerinin Tırnakla Yazılması
- Req8: exclusiveMinimum Yerine Basitleştirme
- Req9: Header'da allowEmptyValue Kullanımı Geçersiz
- Req10: X-Flow-Id Zorunlu mu?
- Req11: /v1/reports/payments/{payment_id}/transactions Kaldırılabilir mi?
- Req12: /v1/reports/refunds Gereksiz Parametrelerin Kaldırılması
- Req13: /v1/webhooks Zorunlu mu?
- Req14: /v1/webhooks/signatures Kaldırılmalı mı?
- Req15: Enum Formatlama Düzeni
- Req16: status alanında maxLength / minLength yerine minimum / maximum
- Req17: Sadece Belirtilen Alanların Kullanımı (Payment Modeli)
- Req18: Decimal Formatın Doğruluğu
- Req21: PaymentProvider yalnızca ZIP mi olacak?
- Req22: Sadece Belirtilen Alanların Kullanımı (Refund Modeli)
- Req23: FifaToken türü yanlış tanımlanmış

### Bekleyen Konular (5):
- Req1: Ortam Sunucularının Eklenmesi
- Req3: Açıklamaların Zenginleştirilmesi
- Req19: PaymentListResponse içinde httpStatus gereksiz olabilir mi?
- Req20: HTTP status code tekrarının anlamlılığı
- Req24: İkinci API Gerekliliği
- Req25: bank_id tipinin string'e dönüştürülme talebi
---

#### 🔹 **Req1: Ortam Sunucularının Eklenmesi**

- **Yorum yapılan kod:**
```yaml
  servers:
    - url: https://ux.mediamarkt.com.tr/OnlinePaymentProvider
```

- **Yorum:**
```
(14.07.2025 - 16:34) gallus-mms:
  Add servers for all available stages (INT, QA, PROD)
```   
- **Durum:** **Bekliyor**

- **Açıklama:** PROD, QA ortamları aşağıdaki gibi eklenmeli. Oluşturulacak olan domainler bekleniyor.

- **Önerilen Güncel Hâli:**

  ```yaml
  servers:
    - url: https://qa.ux.mediamarkt.com.tr/OnlinePaymentProvider
    - url: https://ux.mediamarkt.com.tr/OnlinePaymentProvider
  ```

---

#### 🔹 **Req2: Kullanılmayan Parametrelerin Kaldırılması**

- **Yorum1:**
```
(14.07.2025 - 16:47) gallus-mms:
  DMC requests only with the following parameters: payment_status, min_created_date, max_created_date, page and size. All others are not used on our side and can be removed if not required by someone else
```

- **Durum:** **Çözüldü**

- **Açıklama:** Yalnızca aşağıdaki parametreler bırakıldı:
  - `payment_status`
  - `min_created_date`
  - `max_created_date`
  - `page`
  - `size`
 
- **Yorum2:**
```
(17.07.2025 - 16:50) gallus-mms:
  add property payment_id and payment_transaction_id (because you can query that one also)
```
- **Durum:** **Çözüldü**

- **Açıklama:** Yalnızca aşağıdaki parametreler bırakıldı:
  - `payment_id`
  - `payment_transaction_id`
  - `payment_status`
  - `min_created_date`
  - `max_created_date`
  - `page`
  - `size`
---

#### 🔹 **Req3: Açıklamaların Zenginleştirilmesi**

- **Yorum:**
```
EN:
(14.07.2025 - 16:39) gallus-mms:
  Enhance descriptions for all properties (ID in which system; is it unique; maybe also examples) → for both endpoints (refund and payment)
(15.07.2025 - 09:30) Mehmet Gülenç:
  We’ll enhance the descriptions as part of future improvements.
(17.07.2025 - 16:56)
  We're currently in the review process of a new API. It would be good to have detailed descriptions right now because we're using the API from now on and not sometime in future.
```
```
TR:
(14.07.2025 - 16:39) gallus-mms:
 Tüm özellikler için açıklamaları geliştirin (hangi sistemde ID; benzersiz mi; belki örnekler de) → her iki endpoint için de (refund ve payment)
(15.07.2025 - 09:30) Mehmet Gülenç:
 Açıklamaları gelecekteki iyileştirmelerin bir parçası olarak geliştireceğiz.
(17.07.2025 - 16:56)
 Şu anda yeni bir API'nin inceleme sürecindeyiz. API'yi gelecekte bir zaman değil, şu andan itibaren kullandığımız için şu anda detaylı açıklamalara sahip olmak iyi olurdu.
```

- **Durum:** **Bekliyor**

- **Açıklama:** Açıklamaların detaylandırılması için Ömer abi ile toplantı yapılıp beraber her bir parametre için detaylı açıklama çıkarılacaktır.

---

#### 🔹 **Req4: Gereksiz Description Tekrarlarının Kaldırılması**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204323664

- **Yorum yapılan kod:**
```yaml
description: ID of payment
      schema:
        type: integer
        description: ID of payment
```
- **Yorum:**
```
(14.07.2025 - 12:16) wambobambo:
  Here and all the following: Keep the description at the parameter level and avoid repeating it inside schema unless there’s a very specific reason to override or add detail only relevant to the schema.
```

- **Durum:** **Çözüldü**

- **Açıklama:** `schema.description` alanı kaldırıldı; açıklama sadece parametre seviyesinde tutuldu.

---

#### 🔹 **Req5: Kod Biçimlendirme ve Gereksiz allOf Kullanımı**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204338478
  
- **Yorum yapılan kod:**
```yaml
description: Values expressing the method of collection of a payment
        schema:
          allOf:
          - $ref: '#/components/schemas/PaymentType'
```

- **Yorum:**
```
(14.07.2025 - 12:16) wambobambo:
  EN: I would expect use of 2 spaces per indent level. Also, `description` cannot be on the same level. Why not use this simpler version?
  TR: Girinti seviyesi başına 2 boşluk kullanılmasını beklerdim. Ayrıca, description aynı seviyede olamaz. Neden bu daha basit versiyonu kullanmıyoruz?
```
```yaml
- name: payment_type
  in: query
  description: Values expressing the method of collection of a payment
  schema:
    $ref: '#/components/schemas/PaymentType'
```

- **Durum:** **Çözüldü**

- **Açıklama:** `allOf` kaldırılarak sadeleştirme yapıldı.

---

#### 🔹 **Req6: allOf Kaldırılması (Tüm modellerde)**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204355869

- **Yorum:**
```
(14.07.2025 - 12:31) wambobambo:
  also valid for all other params, get rid of allOf:
(14.07.2025 - 15:09) Mehmet Gülenç:
  Got it, we are removing the allOfs.
```
```yaml
- name: currency
  in: query
  description: Currencies available when trading with API
  schema:
    $ref: '#/components/schemas/Currency'
    nullable: true
```

- **Durum:** **Çözüldü**

- **Açıklama:** `allOf` kullanımı tüm parametrelerden kaldırıldı.

---

#### 🔹 **Req7: Date-Time Örneklerinin Tırnakla Yazılması**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204368188

- **Yorum yapılan kod:**
```yaml
type: string
description: Minimum date that item is created to search
format: date-time
example: 2025-01-01T12:00:00Z
```
- **Yorum:**
```
(14.07.2025 - 12:36) wambobambo:
  example: '2025-01-01T12:00:00Z', always wrap date-time strings in quotes ('...') to avoid YAML parsing errors
```
- **Durum:** **Çözüldü**

- **Açıklama:** Tüm `date-time` örnekleri tırnak içine alındı (örneğin `'2025-01-01T12:00:00Z'`).

---

#### 🔹 **Req8: exclusiveMinimum Yerine Basitleştirme**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204375732

- **Yorum yapılan kod:**
```yaml
required: true
schema:
  minimum: 0
  exclusiveMinimum: true
```

- **Yorum:**
```
(14.07.2025 - 12:38) wambobambo:
  If you use exclusiveMinimum: true, then minimum is not inclusive — so 0 is not allowed. Why not writing it simpler:
```
```yaml
- name: size
  in: query
  description: Page size for pagination
  required: true
  schema:
    type: integer
    format: int32
    minimum: 1
```

- **Durum:** **Çözüldü**

- **Açıklama:** `exclusiveMinimum` kaldırılarak `minimum: 1` kullanıldı.

---

#### 🔹 **Req9: Header'da allowEmptyValue Kullanımı Geçersiz**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204380664

- **Yorum yapılan kod:**
```yaml
    format: int32
- name: Accept-Language
  in: header
  allowEmptyValue: true
```

- **Yorum:**
```
(14.07.2025 - 12:41) wambobambo:
  allowEmptyValue is NOT valid for header parameters in OpenAPI 3.0.1. It’s only allowed for query parameters.
```

- **Durum:** **Çözüldü**

- **Açıklama:** Header parametrelerinden `allowEmptyValue` kaldırıldı.
---

#### 🔹 **Req10: X-Flow-Id Zorunlu mu?**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204433823

- **Yorum yapılan kod:**
```yaml
    type: string
- name: X-Flow-Id
in: header
required: true
```

- **Yorum:**
```
(14.07.2025 - 13:01) wambobambo:
  really required?
(15.07.2025 - 12:37) Mehmet Gülenç:
  According to the API linter, X-Flow-Id is a required header, and removing it causes the YAML to fail the lint check due to the must-provide-required-headers rule.
```

- **Durum:** **Çözüldü**

- **Açıklama:** API linter'a göre `X-Flow-Id` zorunlu. Kaldırıldığında `must-provide-required-headers` kuralı nedeniyle YAML doğrulaması başarısız oluyor.

---

#### 🔹 **Req11: /v1/reports/payments/{payment\_id}/transactions Kaldırılabilir mi?**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204987562

- **Yorum yapılan kod:**
```yaml
              $ref: '#/components/schemas/ErrorResponseModel'
    security:
    - FifaToken: []
/v1/reports/payments/{payment_id}/transactions:
```
  
- **Yorum:**
```
(14.07.2025 - 16:45) gallus-mms:
  Not needed by DMC and can be removed if not required by someone else
(17.07.2025 - 16:46) gallus-mms:
  @pfaefflin-mms Is this endpoint not used on your side?
```

- **Durum:** **Çözüldü**

- **Açıklama:** İlgili endpoint kaldırıldı

---

#### 🔹 **Req12: /v1/reports/refunds Gereksiz Parametrelerin Kaldırılması**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204992154

- **Yorum:**
```
(14.07.2025 - 16:47) gallus-mms:
  DMC requests only with the following parameters: status, min_created_date, max_created_date, page and size.
All others are not used on our side and can be removed if not required by someone else
(15.07.2025 - 15:11) pfaefflin-mms:
  To verify the refund notification, XPay probably needs the payment_id or refund_request_id or similar as parameter
(16.07.2025 - 12:18) wambobambo:
  So payment_id and refund_request_id are still needed
(16.07.2025 - 14:20) Mehmet Gülenç:
  We’re currently waiting for the final decision here
(16.07.2025 - 18:38) Mehmet Gülenç:
  The refund_request_id parameter is not present in the filter here. It wasn’t included in the previous version either.
Please correct me if I’m missing something.
```

- **Durum:** **Çözüldü**

- **Açıklama:** Yalnızca aşağıdaki parametreler bırakıldı:
  - `status`
  - `min_created_date`
  - `max_created_date`
  - `page`
  - `size`

---

#### 🔹 **Req13: /v1/webhooks Zorunlu mu?**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204400064

- **Yorum:**
```
(14.07.2025 - 12:49) wambobambo:
  Who is using this? Really required for first released version?
(16.07.2025 - 12:19) wambobambo:
  still needed?
(16.07.2025 - 13:39) Mehmet Gülenç:
  This service will be used by Craftgate to send notifications to us
```

- **Durum:** **Çözüldü**

- **Açıklama:** Endpoint'in gerekliliği Ömer abi ile görüşüldü, şimdilik bırakılması uygun görüldü.

---

#### 🔹 **Req14: /v1/webhooks/signatures Kaldırılmalı mı?**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204400328

- **Yorum:**
```
(14.07.2025 - 12:49) wambobambo:
  Who is using this? Really required for first released version?
```

- **Durum:** **Çözüldü**

- **Açıklama:** Ömer abi ile görüşülüp `signatures` endpoint'i kaldırıldı.

---

#### 🔹 **Req15: Enum Formatlama Düzeni**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204407143

- **Yorum:**
```
(14.07.2025 - 12:52) wambobambo:
  not better type before x-extensible-enum (also for all others):
```
```yaml
CardType:
  type: string
  x-extensible-enum:
    - CREDIT_CARD
    - DEBIT_CARD
    - PREPAID_CARD
```

- **Durum:** **Çözüldü**

- **Açıklama:** `type: string` satırı `x-extensible-enum`'dan önce konumlandırıldı.

---

#### 🔹 **Req16: status alanında maxLength / minLength yerine minimum / maximum kullanılmalı**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204437742

- **Yorum yapılan kod:**
```yaml
  description: The title of the http status
status:
  maxLength: 600
  minLength: 100
```

- **Yorum:**
```
(14.07.2025 - 13:03) wambobambo:
  maxLength and minLength do not apply to integers. They are for strings. For integers, you should use minimum and maximum.
```

- **Durum:** **Çözüldü**

- **Açıklama:** `status` alanından `maxLength` ve `minLength` kaldırıldı.

---

#### 🔹 **Req17: Sadece Belirtilen Alanların Kullanımı (Payment Modeli)**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2205042049

- **Yorum:**
```
(14.07.2025 - 17:08) wambobambo:
  DMC is using only following properties, from our side all other properties could be removed:
  
  - id
  - currency
  - paidPrice
  - customerOrderNumber
  - conversationId
  - transId
  - createdDate
  - pos#bankId
  - paymentGroup
  - paymentType
  - paymentMethod
  We are not reviewing all other properties.
```
- **Durum:** **Çözüldü**

- **Açıklama:** `Payment` modelinde yalnızca yukarıda listelenen alanlar bırakıldı. Diğer tüm alanlar kaldırıldı.

---

#### 🔹 **Req18: Decimal Formatın Doğruluğu**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204451154

- **Yorum:**
```
(14.07.2025 - 13:08) wambobambo:
  Not sure but format: decimal is not a standard OpenAPI format, should create BigDecimal w/o it
(15.07.2025 - 09:18) Mehmet Gülenç:
  According to the OpenAPI standard, the format should be double. Do you confirm this?
(17.07.2025 - 18:36) wambobambo:
  I thought just remove format: decimal, but ok for now
```

- **Durum:** **Çözüldü**

- **Açıklama:** Format olarak `decimal` yerine `double` kullanımı bekleniyor. Stefan şimdilik sorun olmadığını belirtti.

---

#### 🔹 **Req19: PaymentListResponse içinde httpStatus gereksiz olabilir mi?**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2205193445

- **Yorum yapılan kod:**
```yaml
PaymentListResponse:
  required:
    - code
    - data
  type: object
  properties:
    code:
      type: integer
      description: Http status code
      format: int32
    message:
      type: string
      description: Result message
      nullable: true
    data:
      $ref: '#/components/schemas/PaymentListData'
  additionalProperties: false
```

- **Yorum:**
```
(14.07.2025 - 18:16) wambobambo:
  Not sure if this object is still needed, since error handling is by httpStatus and own error model. paymentListData could be used directly. Duplicating the httpStatus here doesn't make sense.
(15.07.2025 - 01:43) Mehmet Gülenç:
  Would it cause any issues if we keep it this way?
(15.07.2025 - 09:12) wambobambo:
  It would make the structure more readable and if not needed... it just produces boilerplate code.
(15.07.2025 - 09:27) Mehmet Gülenç:
  In the first phase, we prefer to keep it this way since it doesn't block the business flow. We can revisit and discuss it later, as business-level codes might be added to the statusCode field depending on future needs.
(15.07.2025 - 12:23) wambobambo:
  Is it really that difficult to change? the code is a duplicate and instead of returning PaymentListResponse you return PaymentListData, anyway if it causes too much problems changing it now, keep it.
(16.07.2025 - 14:35) Mehmet Gülenç:
  For now, we’d prefer to keep it as it is
(30.07.2025 - 09:22) Florian Heubeck:
  changes most likely would introduce breaking changes. please comply to the api guide and introduce proper error response right from the beginning.
  ```

- **Durum:** **Bekliyor**

- **Açıklama:**
  - (16.07.2025 - 14:35) `httpStatus` ve `data` modelinin ayrıştırılması ileride tekrar değerlendirilecek. Şu anda iş akışını engellemediği için mevcut yapı korunuyor.
  - (30.07.2025 - 09:23) Florian Heubeck'in son comment'i üzerine tekrar değerlendirilecek.

---

#### 🔹 **Req20: HTTP status code tekrarının anlamlılığı**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204110462

- **Yorum:**
```
(14.07.2025 - 10:58) pfaefflin-mms:
  just repeating the http error code is not really helpful
(15.07.2025 - 01:31) Mehmet Gülenç:
  This field is not limited to HTTP status codes — it may also include error codes returned from Craftgate or business-level validations. For now, it only returns the HTTP status code.
(30.07.2025 - 09:23) Florian Heubeck:
  please name it differently then and describe it properly.
```

- **Durum:** **Bekliyor**

- **Açıklama:**
  - (15.07.2025 - 01:31) Şu an yalnızca HTTP status dönüyor; ileride iş seviyesinde hata kodlarının da buraya eklenmesi planlanabilir.
  - (30.07.2025 - 09:23) Florian Heubeck'in son comment'i üzerine tekrar değerlendirilecek.
  - 
---

#### 🔹 **Req21: PaymentProvider yalnızca ZIP mi olacak?**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204113182

- **Yorum yapılan kod:**
```yaml
PaymentProvider:
  x-extensible-enum:
  - ZIP
```

- **Yorum:**
```
(14.07.2025 - 10:59) pfaefflin-mms:
  is ZIP the only available payment provider?
(14.07.2025 - 14:47) Mehmet Gülenç:
  Denizbank and Garanti Bank will be included, but we’ll start with ZIP first.
```

- **Durum:** **Çözüldü**

- **Açıklama:** Thomas'ın sorusu cevaplandı. İlk aşamada yalnızca ZIP gösterilecek, ilerleyen aşamalarda diğer sağlayıcılar eklenecek.

---

#### 🔹 **Req22: Sadece Belirtilen Alanların Kullanımı (Refund Modeli)**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2205046941

- **Yorum:**
```
(14.07.2025 - 17:10) wambobambo:
  DMC is using only following properties, from our side all other properties could be removed:

  - id
  - currency
  - refundPrice
  - customerOrderNumber
  - conversationId
  - transId
  - createdDate
  - pos#bankId
  - paymentGroup
  - paymentType
  - paymentMethod

  We are not reviewing all other properties.
```

- **Durum:** **Çözüldü**

- **Açıklama:** `Refund` modelinde yalnızca yukarıda listelenen alanlar bırakıldı. Diğer tüm alanlar kaldırıldı.

#### 🔹 **Req23: FifaToken türü yanlış tanımlanmış**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2204396209

- **Yorum yapılan kod:**
```yaml
securitySchemes:
    FifaToken:
      type: apiKey
```

- **Yorum:**

```
(14.07.2025 - 12:47) wambobambo:
  isn't it:
```
```yaml
FifaToken:
  type: http
  scheme: bearer
  bearerFormat: JWT
  description: Token issued by FIFA for authorization

and not type: apiKey
```

- **Durum:** **Çözüldü**

- **Açıklama:** Güvenlik şemasında FifaToken örnekte gösterildiği gibi JWT schema olarak güncellendi

---

#### 🔹 **Req24: İkinci API Gerekliliği**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2236549987

- **Yorum:**
```
(28.07.2025 - 16:45) Tung Beier:
  do we really need a second api for reporting or can these reporting paths also be added to the other api spec?
```

- **Durum:** **Bekliyor**

- **Açıklama:** Reporting endpoint'lerinin ayrı bir API olarak mı yoksa mevcut API spec'e eklenmiş olarak mı tutulacağına dair Ömer abi'den karar bekleniyor.

---

#### 🔹 **Req25: bank_id tipinin string'e dönüştürülme talebi**
- **PR Link:** https://github.com/MediaMarktSaturn/oas/pull/1666#discussion_r2241639153

- **Yorum:**
```
(30.07.2025 - 09:19) Florian Heubeck:
  string
```

- **Durum:** **Bekliyor**

- **Açıklama:** Ömer abi'ye sorulacak.

---
