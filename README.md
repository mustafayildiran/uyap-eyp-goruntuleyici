# 📦 UYAP EYP Görüntüleyici

[![Sürüm](https://img.shields.io/badge/sürüm-1.1.5-blue)](CHANGELOG.md)
[![Lisans: MIT](https://img.shields.io/badge/lisans-MIT-green)](LICENSE)
[![Chrome / Edge](https://img.shields.io/badge/Chrome%20%7C%20Edge-MV3-orange)]()
[![Türkçe](https://img.shields.io/badge/dil-Türkçe-red)]()

`avukat.uyap.gov.tr` üzerinde **`.eyp`** dosyasına tıklayınca çıkan

> **Desteklenmeyen belge türü. Belgeyi bilgisayara indirerek görüntüleme işlemi yapabilirsiniz**

hatasına takılmadan dosyayı **sayfa içinde, indirme yapmadan** görüntüleyen Chrome / Edge eklentisi (Manifest V3).

## ✨ Özellikler

- **Tek tık önizleme** — `.eyp` satırlarındaki **👁 Görüntüle** düğmesi üst yazıyı anında açar.
- **Gerçek EYP çözümleme** — `UstYazi/ustyazi.pdf`, `Ekler/*`, `Ustveri.xml` (konu, gönderen kurum, belge no, tarih), imza/mühür kanıtları.
- **İç içe EYP** — ekin içindeki `.eyp` tek tıkla açılır (`📦 İçini aç`), `← Geri` ile dönülür.
- **UYAP evrak paketi** — `evrak_*.zip` (`ana_evrak.pdf` + ekler) ve **`.udf`** dosyaları da açılır.
- **Ham PDF toleransı** — UYAP bazen `.eyp` adıyla ham PDF sunar; eklenti uzantıya değil içeriğe (`%PDF-`) bakıp doğrudan açar.
- **Sıfır indirme** — dosya baytları arka planda yakalanır; başlayan indirme otomatik iptal edilir, Downloads klasörüne dokunulmaz.
- **%100 yerel** — çözümleme tamamen tarayıcıda (JSZip); hiçbir dosya sunucuya gönderilmez.
- **Eklentisiz mod** — `viewer.html` tek başına sürükle-bırak görüntüleyici olarak çalışır.

## 🖼️ Ekran görüntüleri

> `assets/` klasörüne `gorunum-1.png`, `gorunum-2.png` ekleyin, aşağıdaki satırlar otomatik çalışır.

![Üst yazı önizleme](assets/gorunum-1.png)
![Ekler ve iç içe EYP](assets/gorunum-2.png)

## 🚀 Kurulum

**Seçenek A — Mağaza (önerilen):** *yayınlanınca bağlantı buraya eklenecek.*

**Seçenek B — Geliştirici modu:**

1. `chrome://extensions` → sağ üstte **Geliştirici modu**nu açın.
2. **Paketlenmemiş öğe yükle** → bu klasörü seçin.
3. `avukat.uyap.gov.tr` sekmesini yenileyin.

Ayrıntılı adımlar için: [KURULUM.md](KURULUM.md)

## 🖱️ Kullanım

1. UYAP'ta dosya listesindeki `.eyp` satırında **👁 Görüntüle**'ye tıklayın.
2. Üst yazı PDF'i otomatik açılır; ekler soldadır.
3. `.eyp` uzantılı ekte **📦 İçini aç**, dönüşte **← Geri**.
4. Hata diyaloğu (`Desteklenmeyen belge türü`) çıkarsa içindeki **EYP Görüntüleyici ile açmayı dene** düğmesini kullanın.

## 🧠 Nasıl çalışıyor?

UYAP sayfada gerçek `.eyp` bağlantısı vermez (JSF/PrimeFaces arka plan istekleri kullanır) ve content script'ler sayfanın isteklerini göremez (izole dünya). Eklenti üç katmanla çözer:

| Katman | Dosya | Görev |
|---|---|---|
| Sayfa kancası (`world: MAIN`) | [`inject.js`](inject.js) | `fetch` / `XHR` / `blob:` yanıtlarını koklar, ZIP bulursa baytları aktarır |
| Sayfa betiği (izole dünya) | [`content.js`](content.js) | Düğmeleri ekler, modal görüntüleyiciyi çizer, isteği tetikler |
| Arka plan | [`background.js`](background.js) | Başlayan indirimi iptal edip isteği `fetch` ile tekrarlar |
| Ortak çözümleyici | [`eyp-parser.js`](eyp-parser.js) | EYP / UYAP evrak paketi / UDF / ham PDF ayrımı + üstveri okuma |

Tanı için konsolda: `__eypDebug()` ve `__eypNetCache()`

## 📁 Dosya yapısı

```
├── manifest.json      # MV3 bildirimi (yalnızca *.uyap.gov.tr)
├── inject.js          # MAIN world ağ kancası
├── content.js         # UYAP sayfa betiği + modal
├── eyp-parser.js      # Ortak EYP/UDF/ZIP çözümleyici
├── background.js      # İndirme iptali + istek tekrarı
├── viewer.html/js     # Bağımsız sürükle-bırak görüntüleyici
├── viewer.css         # Modal stilleri
├── popup.html         # Araç çubuğu penceresi
├── jszip.min.js       # Yerel ZIP kütüphanesi (CDN yok, MV3 uyumlu)
├── KURULUM.md         # Son kullanıcı kurulum kılavuzu
├── CHANGELOG.md       # Sürüm notları
└── LICENSE            # MIT
```

## 🔒 Gizlilik

Eklentinin sunucusu, hesabı ve analitik kodu yoktur. Dosyalar yalnızca tarayıcınızda çözümlenir; avukat profili dahil hiçbir veri cihazdan çıkmaz.

## ⚠️ Önemli not

Bu eklenti **resmî UYAP ürünü değildir**; paket *içeriğini görüntülemeye* yarar, **imza doğrulaması yapmaz**. Hukuki geçerlilik değerlendirmesi için İmzager / resmî doğrulama araçlarını kullanın. Matbu-bilgi hatası içermez; dosya UYAP'tan geldiği gibi gösterilir.

## 🤝 Katkı

Hata bildirimi ve öneriler için **Issues** açın. PR'lar memnuniyetle karşılanır:

1. Fork → dal (`ozellik/...`) → commit → PR
2. UYAP DOMRA değişikliklerinde ilgili HTML/Network örneğini (kişisel verisiz) ekleyin

## 📬 İletişim

Av. Mustafa Yıldıran — av.mustafayildiran@gmail.com

## 📄 Lisans

[MIT](LICENSE) © 2026 Mustafa Yıldıran
