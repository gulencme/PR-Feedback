## Pull Request Feedback Summary


### PROVIDER

#### 🔹 **Req1: Ortam Sunucularının Eklenmesi**

- **Yorum:**

  Add servers for all available stages (INT, QA, PROD)
    ```yaml
  servers:
    - url: https://ux.mediamarkt.com.tr/OnlinePaymentProvider
  ```

- **Durum:** **Bekliyor**

- **Açıklama:** Yeni domainlerin iletilmesi Ömer abi tarafında bekliyor.

- **Önerilen Güncel Hâli:**

  ```yaml
  servers:
    - url: https://qa.ux.mediamarkt.com.tr/OnlinePaymentProvider
    - url: https://ux.mediamarkt.com.tr/OnlinePaymentProvider
  ```

---

#### 🔹 **Req2: Kullanılmayan Parametrelerin Kaldırılması**

- **Yorum:**

  DMC requests only with the following parameters: payment_status, min_created_date, max_created_date, page and size. All others are not used on our side and can be removed if not required by someone else

- **Durum:** **Çözüldü**

- **Açıklama:** Yalnızca aşağıdaki parametreler bırakıldı:

  - `payment_status`
  - `min_created_date`
  - `max_created_date`
  - `page`
  - `size`

---

#### 🔹 **Req3: Açıklamaların Zenginleştirilmesi**

- **Yorum:**

  Enhance descriptions for all properties (ID in which system; is it unique; maybe also examples) → for both endpoints (refund and payment)

- **Durum:** **Çözülmedi**

- **Açıklama:** Açıklamaların detaylandırılması gelecekte yapılacak iyileştirmeler kapsamında ele alınacaktır.

---

#### 🔹 **Req4: Gereksiz Description Tekrarlarının Kaldırılması**

- **Yorum:**

  Keep the description at the parameter level and avoid repeating it inside schema unless there’s a very specific reason to override or add detail only relevant to the schema.

- **Durum:** **Çözüldü**

- **Açıklama:** `schema.description` alanı kaldırıldı; açıklama sadece parametre seviyesinde tutuldu.

---

#### 🔹 **Req5: Kod Biçimlendirme ve Gereksiz allOf Kullanımı**

- **Yorum:** **Çözüldü**

  I would expect use of 2 spaces per indent level. Also, `description` cannot be on the same level. Why not use this simpler version?

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

- **Yorum:**

  Also valid for all other params, get rid of allOf:

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

- **Yorum:**

  Always wrap date-time strings in quotes (`'...'`) to avoid YAML parsing errors

- **Durum:** **Çözüldü**

- **Açıklama:** Tüm `date-time` örnekleri tırnak içine alındı (örneğin `'2025-01-01T12:00:00Z'`).

---

#### 🔹 **Req8: exclusiveMinimum Yerine Basitleştirme**

- **Yorum:**

  `exclusiveMinimum: true` yerine doğrudan `minimum: 1` kullanılmalı.

  ```yaml
  schema:
    type: integer
    format: int32
    minimum: 1
  ```

- **Durum:** **Çözüldü**

- **Açıklama:** `exclusiveMinimum` kaldırılarak `minimum: 1` kullanıldı.

---

#### 🔹 **Req9: Header'da allowEmptyValue Kullanımı Geçersiz**

- **Yorum:**

  `allowEmptyValue` is not valid for header parameters. Just omit `required` if it's optional.

- **Durum:** **Çözüldü**

- **Açıklama:** Header parametrelerinden `allowEmptyValue` kaldırıldı.

---

#### 🔹 **Req10: X-Flow-Id Zorunlu mu?**

- **Yorum:**

  Really required?

- **Durum:** **Cevaplandı**

- **Açıklama:** API linter'a göre `X-Flow-Id` zorunlu. Kaldırıldığında `must-provide-required-headers` kuralı nedeniyle YAML doğrulaması başarısız oluyor.

---

#### 🔹 **Req11: /v1/reports/payments/{payment\_id}/transactions Kaldırılabilir mi?**

- **Yorum:**

  Not needed by DMC and can be removed if not required by someone else

- **Durum:** **Çözüldü**

- **Açıklama:** İlgili endpoint kaldırıldı

---

#### 🔹 **Req12: /v1/reports/refunds Gereksiz Parametrelerin Kaldırılması**

- **Yorum:**

  DMC requests only with the following parameters: `status`, `min_created_date`, `max_created_date`, `page`, `size`. All others can be removed.

- **Durum:** **Çözüldü**

- **Açıklama:** Belirtilen parametreler dışındaki tüm parametreler kaldırıldı.

---

#### 🔹 **Req13: /v1/webhooks Zorunlu mu?**

- **Yorum:**

  Who is using this? Really required for first released version?

- **Durum:** **Cevaplandı**

- **Açıklama:** Endpoint'in gerekliliği Ömer abi ile görüşüldü, şimdilik bırakılması uygun görüldü.

---

#### 🔹 **Req14: /v1/webhooks/signatures Kaldırılmalı mı?**

- **Yorum:**

  Who is using this? Really required for first released version?

- **Durum:** **Çözüldü**

- **Açıklama:** `signatures` endpoint'i kaldırıldı.

---

#### 🔹 **Req15: Enum Formatlama Düzeni**

- **Yorum:**

  Not better to place `type: string` before `x-extensible-enum`?

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

#### 🔹 **Req16: status alanında maxLength / minLength yerine minimum / maximum kullanılmalı**

- **Yorum:**

  `maxLength` and `minLength` are only valid for strings. For integers, you should use `minimum` and `maximum`.

- **Durum:** **Çözüldü**

- **Açıklama:** `status` alanından `maxLength` ve `minLength` kaldırıldı.

#### 🔹 **Req17: Sadece Belirtilen Alanların Kullanımı (Payment Modeli)**

- **Yorum:**

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

- **Durum:** **Çözüldü**

- **Açıklama:** `Payment` modelinde yalnızca yukarıda listelenen alanlar bırakıldı. Diğer tüm alanlar kaldırıldı.

#### 🔹 **Req18: Decimal Formatın Doğruluğu**

- **Yorum:**

  Format: `decimal` is not a standard OpenAPI format. Should create BigDecimal without using it. According to OpenAPI standard, format should be `double`. Do you confirm?

- **Durum:** **Bekleniyor**

- **Açıklama:** Format olarak `decimal` yerine `double` kullanımı bekleniyor olabilir. Karar için Stefanın cevabı bekleniyor.

#### 🔹 **Req19: PaymentListResponse içinde httpStatus gereksiz olabilir mi?**

- **Yorum:**

  ```
  stefan:
    Not sure if this object is still needed, since error handling is by httpStatus and own error model. paymentListData could be used directly. Duplicating the httpStatus here doesn't make sense.

  mehmet:
    Would it cause any issues if we keep it this way?

  stefan:
    It would make the structure more readable and if not needed... it just produces boilerplate code.

  mehmet:
    In the first phase, we prefer to keep it this way since it doesn't block the business flow. We can revisit and discuss it later, as business-level codes might be added to the statusCode field depending on future needs.
  ```

- **Durum:** **Çözülmedi**

- **Açıklama:** `httpStatus` ve `data` modelinin ayrıştırılması ileride tekrar değerlendirilecek. Şu anda iş akışını engellemediği için mevcut yapı korunuyor.

#### 🔹 **Req20: HTTP status code tekrarının anlamlılığı**

- **Yorum:**

  ```
  pfaefflin-mms:
    just repeating the http error code is not really helpful

  mehmet:
    This field is not limited to HTTP status codes — it may also include error codes returned from Craftgate or business-level validations. For now, it only returns the HTTP status code.
  ```

- **Durum:** **Çözülmedi**

- **Açıklama:** Şu an yalnızca HTTP status dönüyor; ileride iş seviyesinde hata kodlarının da buraya eklenmesi planlanabilir.

#### 🔹 **Req21: PaymentProvider yalnızca ZIP mi olacak?**

- **Yorum:**

  ```
  pfaefflin-mms:
    is ZIP the only available payment provider?

  gulencme:
    Denizbank and Garanti Bank will be included, but we’ll start with ZIP first.
  ```

- **Durum:** **Cevaplandı**

- **Açıklama:** İlk aşamada yalnızca ZIP gösterilecek, ilerleyen aşamalarda diğer sağlayıcılar eklenecek.

---

#### 🔹 **Req22: Sadece Belirtilen Alanların Kullanımı (Refund Modeli)**

- **Yorum:**

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

- **Durum:** **Çözüldü**

- **Açıklama:** `Refund` modelinde yalnızca yukarıda listelenen alanlar bırakıldı. Diğer tüm alanlar kaldırıldı.

#### 🔹 **Req23: FifaToken türü yanlış tanımlanmış**

- **Yorum:**

  ```
  wambobambo:
    isn't it:

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

