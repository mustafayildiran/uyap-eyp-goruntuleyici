# Değişiklik günlüğü

## 1.1.5
- İçerik tabanlı PDF tespiti: `.eyp` adıyla sunulan ham PDF (`%PDF-`) doğrudan açılır.
- Blob MIME türleri düzeltildi (PDF ham metin dökülmüyor).

## 1.1.4
- `storage.session` ile kurma bilgisi (uyuyan service worker'a dayanıklı).
- Tetikleme `node.click()` ile (orijinal UYAP akışı birebir çalışır).
- Sayfa kancası yoklama ping'i (eski sürümde kalındıysa açık uyarı).
- Tanı çıktısına kanca/bekleyen/yakalanan durumu eklendi.

## 1.1.3
- Kör GET 404'leri kapatıldı (POST ucu bilinirken GET atılmaz).
- Hata mesajlarında sorunlu adres + "Tanıyı kopyala" düğmesi.
- `__eypDebug()` tanı günlüğü; `openBuffer`/`render` hata kaydı.

## 1.1.2
- `world: MAIN` kanca (`inject.js`): fetch/XHR/blob yanıt koklama.
- Background indirme iptali + POST tekrarı (sıfır indirme akışı).
- `document_start` + `all_frames`.

## 1.1.0
- UYAP evrak paketi (`ana_evrak.pdf` + ekler) ve UDF desteği.
- Hata diyaloğu düğmesi, `📦 EYP Aç` yüzen düğme.

## 1.0.0
- İlk sürüm: `.eyp` bağlantı yakalama + modal önizleme.
