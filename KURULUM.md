# UYAP EYP Görüntüleyici (Chrome / Edge eklentisi)

avukat.uyap.gov.tr üzerinde `.eyp` dosyasına tıklayınca çıkan

> **Desteklenmeyen belge türü. Belgeyi bilgisayara indirerek görüntüleme işlemi yapabilirsiniz**

hatasına takılmadan dosyayı sayfa içinde görüntüleyin.

## Ne yapar?

- Sayfadaki `.eyp` bağlantılarının yanına **👁 Görüntüle** düğmesi ekler.
- Tıklayınca dosyayı aynı oturumla indirip (`credentials: include`) sayfa içinde modal pencerede açar:
  - **Üst yazı PDF** (UstYazi/ustyazi.pdf) doğrudan önizleme
  - **Ekler** listesi (Ekler/EK-*.pdf ve iç içe `.eyp` dosyaları — tek tıkla alt pakete inilir)
  - **Üstveri**: Konu, gönderen kurum, belge no, tarih
  - İmza/mühür kanıtlarının (`Imzalar/`, `Muhur/`) varlığı
- `evrak_*.zip` (UYAP evrak paketi: `ana_evrak.pdf` + ekler) ve `.udf` dosyalarını da açar.
- Hata diyaloğu çıkarsa (`Desteklenmeyen belge türü`) içine **“EYP Görüntüleyici ile açmayı dene”** düğmesi ekler.
- Sağ altta her zaman **📦 EYP Aç** düğmesi vardır: indirdiğiniz dosyayı seçip görüntüleyebilirsiniz.
- Tüm çözümleme **yerelde** yapılır (JSZip). Dosya sunucuya gönderilmez.

Gerçek dosyanızla doğrulandı:
- `evrak_13357584567.zip` → UYAP evrak paketi → içinden `(Ek 1)EK-1.eyp` → konu, üst yazı, 3 ek başarıyla çözümlendi.
- `evrak_13357584553.zip` → iç EYP (eksiz, yalnızca üst yazı) çözümlendi.
- `evrak_10064990023.udf` → UDF olarak çözümlendi.

## Kurulum (2 dakika)

1. Chrome / Edge adres çubuğuna yazın: `chrome://extensions`
2. Sağ üstte **Geliştirici modu**nu açın.
3. **Paketlenmemiş öğe yükle** → bu klasörü seçin (`EYP Okuma`).
4. `avukat.uyap.gov.tr` sekmesini yenileyin.

Eklentisiz denemek isterseniz: bu klasördeki `viewer.html` dosyasına çift tıklayın,
`.eyp` / `evrak_*.zip` dosyasını sürükleyip bırakın.

## Kullanım — indirme YOK, doğrudan açılır

1. UYAP'ta `.eyp` satırındaki **👁 Görüntüle**'ye tıklayın.
2. Eklenti UYAP'ın kendi isteğini tetikler, dosya baytlarını arka planda yakalar
   (gerekirse başlayan indirimi otomatik iptal eder) ve modal pencerede
   üst yazı + ekler + üstveriyi açar. Downloads klasörüne dokunmazsınız.
3. İç içe `.eyp` ekte **📦 İçini aç**, dönüşte **← Geri**.

> v1.1.0: “dosya seç” yedeği ana akıştan çıkarıldı. content script izole dünyada
> çalıştığı için sayfanın isteklerini göremiyordu — artık `inject.js`
> (`world: MAIN`) fetch/XHR/blob yanıtlarını koklar, `background.js`
> başlayan indirimi iptal edip isteği fetch ile tekrarlar.

### Eklentiyi güncelleme

`chrome://extensions` → UYAP EYP Görüntüleyici kartındaki **yenile (↻)** düğmesine basın,
ardından UYAP sekmesini yenileyin. Yeni izin (webRequest) istenirse onaylayın.

## Sınırlar / Notlar

- UYAP bazı dosyaları POST + evrak ID ile indirir (doğrudan GET bağlantısı yoktur). Bu durumda otomatik yakalama
  mümkün olmaz; eklenti sizi indirme + **EYP Aç** akışına yönlendirir.
- `.tif` ekleri tarayıcıda önizlenemez, **İndir** düğmesi verilir.
- İmza **doğrulaması** yapılmaz; yalnızca paket içeriği gösterilir. Hukuki geçerlilik için İmzager / V-İmza
  doğrulamasını kullanın.
- Çok büyük eklerde (ör. 10+ MB taramalar) ilk açılış birkaç saniye sürebilir.

## Dosyalar

- `manifest.json` — MV3 bildirimi (yalnızca `*.uyap.gov.tr` + downloads/storage)
- `content.js` — UYAP sayfasına gömülen yakalayıcı + modal
- `eyp-parser.js` — ortak EYP/UYAP-zip/UDF çözümleyici (JSZip tabanlı)
- `viewer.html` + `viewer.js` — bağımsız sürükle-bırak görüntüleyici (eklentisiz de çalışır)
- `jszip.min.js` — yerelde paketlenmiş ZIP kütüphanesi (CDN yok, MV3 uyumlu)
- `background.js`, `popup.html`, `viewer.css`
