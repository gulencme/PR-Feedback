## Gateway Payment Api Project Pull Request Feedback Summary

**📅 Son Güncelleme:** 04 Ağustos 2025 - 08:45

## 📊 Durum Özeti

| **Durum** | **Sayı** |
|-----------|----------|
| ✅ **Çözüldü** | 9 |
| ⏳ **Bekliyor** | 9 |
| **📝 Toplam** | **18** |

---

### Çözülen Konular (9):
- Req1: X-Api-Key Zorunluluğu
- Req2: LineItemId Format Değişikliği  
- Req3: Quantity Minimum Değeri
- Req4: Unit Price Money Object Kullanımı
- Req7: Postal Code Zorunluluğu
- Req9: FulfillmentMethod Enum Değerleri
- Req12: Product ID Açıklaması
- Req15: Yabancı Para Birimi
- Req17: RefundResultModel altında status enum
- Req18: SellerType Enum Değişikliği

### Bekleyen Konular (9):
- Req5: Model Suffix Kaldırılması
- Req6: Customer ID Sistem Açıklaması
- Req8: Error Data Type Belirlenmesi
- Req10: Detaylı Proje Açıklaması
- Req11: Versiyonlama Server URL'de
- Req13: Product Group ID Tip Değişikliği
- Req14: Payment ID String Yapılması
- Req16: ItemType Adlandırma Değişikliği

---

## Provider Project Pull Request Feedback Summary

**📅 Son Güncelleme:** 01 Ağustos 2025 - 10:44

## 📊 Durum Özeti

| **Durum** | **Sayı** |
|-----------|----------|
| ✅ **Çözüldü** | 22 |
| ⏳ **Bekliyor** | 4 |
| **📝 Toplam** | **26** |

---

### Çözülen Konular (22):
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
- Req24: İkinci API Gerekliliği
- Req25: bank_id tipinin string'e dönüştürülme talebi

### Bekleyen Konular (4):
- Req1: Ortam Sunucularının Eklenmesi
- Req3: Açıklamaların Zenginleştirilmesi
- Req19: PaymentListResponse içinde httpStatus gereksiz olabilir mi?
- Req20: HTTP status code tekrarının anlamlılığı
