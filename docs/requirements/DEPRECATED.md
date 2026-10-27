# Deprecated Requirements

Bu belge, daha önce kabul edilmiş ancak sonradan yürürlükten kaldırılmış gereksinimleri ve bunların neden geçersiz hâle geldiğini kaydeder.

Her gereksinimin özgün metni, eski `REQ` kimliği ve deprecation gerekçesi tarihsel izlenebilirliği korumak amacıyla muhafaza edilir.

Yürürlükten kaldırılmış gereksinimler güncel ürün davranışını tanımlamak için kullanılmamalıdır. Bunların yerine geçen gereksinimler varsa ilgili yeni `REQ` kimliği belirtilir.

---

## REQ-164 — Beklemede Durumunda Timer Başlatma Kısıtı

**Status:** DEPRECATED

Task Status ile Timer State birbirinden bağımsız kavramlar olmakla birlikte, veri bütünlüğünü korumak amacıyla bazı durum kombinasyonları sistem tarafından kısıtlanır.

Bir Task'ın Status'u `Beklemede` ise çalışma timer'ı başlatılamaz.

Kullanıcının Task üzerinde yeniden çalışmaya başlayabilmesi için öncelikle:

1. Task Status'u `Beklemede` durumundan `Devam Ediyor` durumuna alınır.
2. Ardından `Başlat / Devam Et` eylemi kullanılabilir.
3. Çalışma timer'ı bu eylemden sonra başlar.

Bekleme sona erdiğinde Task'ın çalışma timer'ı otomatik olarak başlamaz.

Normal `Duraklat / Beklet` durumunda ise Task Status'u `Devam Ediyor` olarak kalabilir ve kullanıcı doğrudan `Başlat / Devam Et` ile timer'ı yeniden çalıştırabilir.

Bu kısıtlama, Task Status ile Timer State arasındaki kavramsal ayrımı korurken veri bütünlüğünü sağlamak amacıyla uygulanır.

Temel prensip:

> Kullanıcıya esneklik, veriye disiplin.

**Deprecation Note:**

Bu gereksinim, `REQ-215 — Beklemede Olan Task'ı Tek Eylemle Devam Ettirme` gereksinimiyle değiştirilmiştir.

REQ-164, Task Status `Beklemede` durumundan `Devam Ediyor` durumuna geçirildikten sonra çalışma timer'ının ayrı bir eylemle başlatılmasını öngörüyordu. Kabul edilen REQ-215 ise Task Status değişikliğinin ve çalışma döneminin başlatılmasının tek bir `Devam Et` Command'ı kapsamında atomik olarak gerçekleştirilmesini gerektirir.

Yeni davranış için REQ-215 esas alınmalıdır.

---