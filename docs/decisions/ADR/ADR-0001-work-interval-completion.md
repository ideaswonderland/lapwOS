## ADR / Requirement Decision — Work Interval'ın Tamamlanması

**Status:** ACCEPTED

Bir Task çalışırken `Tamamla` Command'ı uygulandığında, aktif çalışma dönemi `WORK_PAUSED` event'i ile sonlandırılır ve Task'ın tamamlanması `TASK_COMPLETED` event'i ile kaydedilir.

Her iki event aynı Command tarafından oluşturuluyorsa tek bir atomik transaction içinde kaydedilmelidir.

### Kurallar

1. `WORK_PAUSED`, aktif Work Interval'ı kapatır.
2. `TASK_COMPLETED`, Task'ın tamamlanma olayını kaydeder; Work Interval'ı ikinci kez kapatmaz.
3. Her iki event'in kaydı ya birlikte başarılı olur ya da birlikte başarısız olur.
4. Aktif Work Interval yoksa, `TASK_COMPLETED` yalnızca tamamlanma kaydını oluşturur; gereksiz bir `WORK_PAUSED` event'i üretilmez.
5. Work Interval türetme katmanı, aynı çalışma döneminin iki kez kapatılmasını veya süresinin iki kez hesaplanmasını engellemelidir.