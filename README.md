<p align="center">
  <img src="https://github.com/Gakkosh/sakura-encode/raw/gorseller/logo.png" alt="Sakura Encode" width="360">
</p>

<h1 align="center">Sakura Encode</h1>

<p align="center">
  Altyazı gömme ve toplu video kodlama uygulaması
</p>

<p align="center">
  <a href="https://github.com/Gakkosh/sakura-encode/releases/latest"><b>İndir</b></a> ·
  <a href="#özellikler">Özellikler</a> ·
  <a href="#kurulum">Kurulum</a> ·
  <a href="https://github.com/Gakkosh/sakura-encode/releases">Sürüm notları</a> ·
  <a href="#kullanım-koşulları">Kullanım koşulları</a>
</p>

---

Sakura Encode, çevirisi biten bir bölümü yayına hazır videoya dönüştürür. Altyazıyı videoya kalıcı olarak gömer (hardsub), istenirse başına intro ekler, istenmeyen bölümleri keser, filigran koyar ve videoyu rahat oynayacak boyuta küçültür. Tek bir film de verilebilir, bir sezonun bütün bölümleri de: program bölümleri altyazılarıyla kendisi eşleştirir, sırayla kodlar ve her çıktıyı teslimden önce kendisi kontrol eder.

<p align="center">
  <img src="https://github.com/Gakkosh/sakura-encode/raw/gorseller/ekran/kuyruk.png" alt="Kuyruk: bölümler sırayla kodlanıyor" width="860">
</p>

## Tasarım hedefleri

Uygulama dört hedef üzerine kuruldu:

| Hedef | Nasıl sağlanıyor |
|---|---|
| **Her durumda kodlasın** | Sorun çıkarsa program aynı kodlayıcıyla daha güvenli yöntemleri sırayla dener (ekran kartıyla çözme → işlemciyle çözme → güvenli mod). 10-bit HEVC, FLAC ses, çift ses izli MKV gibi zor kaynaklarla denendi. |
| **Seçilen ayar kullanılsın** | Seçilen kodlayıcı (örneğin ekran kartı) hiçbir durumda kendiliğinden değiştirilmez; boyut ve kalite ayarları olduğu gibi uygulanır. |
| **Bilgisayar kasmasın** | Kodlama düşük öncelikle çalışır, video ekran kartıyla çözülür, istenirse işlemci kullanımı yarıya ya da çeyreğe indirilir, kodlama tek tuşla duraklatılır. |
| **Toplu işte her şey elde olsun** | Her dosyanın adı, altyazısı, ses izi, introsu ve kodlama ayarı ayrı ayrı değiştirilebilir; değiştirilmeyenler genel ayarla gelir. |

**Ölçülen örnekler** (NVIDIA RTX 4060):

- 1 sa 21 dk'lık, 4,31 GB'lık 10-bit HEVC film, intro eklenerek: **7 dk 40 sn**'de **1,51 GB**; 118.153 karenin tamamı yerinde.
- 24 dakikalık 1080p bölüm 2500 kbps ile: yaklaşık **450 MB**.
- Kurulum dosyası: **147 MB**; ffmpeg içinde gelir, ayrıca bir şey kurmak gerekmez.

## Özellikler

### Toplu iş
- Videoları ve altyazıları pencereye sürükleyip bırakmak yeterli; altyazılar bölüm numarasına göre eşleşir (`S02E12`, `Bölüm 12`, `- 12`, `[12]` gibi bütün yaygın yazımlar).
- Kuyruk kaydedilir: program kapanıp açılsa da liste yerinde durur.
- Her dosyanın çıktı adı kuyrukta tıklanarak değiştirilir; ad verilmezse video kendi adıyla kaydedilir.
- Her dosya genel ayarlarla gelir; istenen dosyaya ayrıca farklı bitrate, kodlayıcı ya da kapsayıcı verilebilir.

### Altyazı ve yazı tipleri
- ASS/SSA altyazılar bütün stilleriyle gömülür.
- Altyazıdaki her yazı tipi aranır (videoya gömülü, eklenen klasörler, bilgisayarda kurulu). Eksik yazı tipleri ve yazı tipinde bulunmayan Türkçe harfler (ş, ğ, ı, İ) kodlamadan **önce** gösterilir.
- Canlı önizleme: video kodlanmadan, çıktının son hâliyle oynatılır. Intro baştadır, kesilen aralıklar atlanır, altyazı ve filigran yerindedir; an salisesine kadar seçilir, kare kare ilerlenir.
- Videonun kendi altyazıları sessizce kaybolmaz: altyazı dosyası verilmişse çıkarılır, verilmemişse seçmeli altyazı olarak korunur.

### Intro
- Tek tıkla bütün kuyruğa intro eklenir; dosya bazında değiştirilebilir ya da kaldırılabilir.
- Intro da video da kendi boyutunda kalır; hiçbiri kırpılmaz ya da bozulmaz. Kare hızı ve ses otomatik uyarlanır.

### Kırpma ve filigran
- Videodan istenen aralıklar çıkarılır: baştaki yayıncı logosu, sondaki tanıtım ya da aradaki bir bölüm. Kesim kodlamanın içinde yapılır, video ikinci kez sıkıştırılmaz; gömülen altyazı kaymaz.
- Kesimin iki yanındaki kareler önizlenir; süre kare kare ayarlanır, önizlemede bulunan an tek tıkla kesime çevrilir.
- Filigran: logo sürüklenerek yerleştirilir, boyutu ve görünürlüğü ayarlanır; sürekli ya da belli aralıklarla gösterilir. Logonun çevresindeki saydam boşluk kendiliğinden kırpılır.

### Kodlama
- NVIDIA, Intel ve AMD ekran kartı kodlayıcıları (H.264, HEVC, AV1) ve işlemci kodlayıcıları (x264, x265, SVT-AV1). Program açılışta bilgisayarda gerçekten çalışanları sınar.
- Yedi basamaklı hız ayarı (superfast … veryslow karşılıkları).
- Üç boyut yöntemi: sabit bitrate, hedef dosya boyutu (MB) ya da sabit kalite.
- Ses: AAC, Opus ya da olduğu gibi kopyalama. MP4 ve MKV çıktı.

### Güvenilirlik
- Kodlanan dosya bitene kadar geçici adla yazılır; yarım kalmış bir dosya hiçbir zaman bitmiş gibi görünmez. Elektrik kesilirse yarım dosya bir sonraki açılışta silinir.
- Kaynak video hiçbir ayarda ezilmez.
- Kodlamaya başlamadan hedef diskteki boş yer kontrol edilir.
- Çıktının adı sonradan Windows'ta değiştirilse bile program dosyayı tanır.

<p align="center">
  <img src="https://github.com/Gakkosh/sakura-encode/raw/gorseller/ekran/sonuc.png" alt="Sonuç kontrolü" width="860">
</p>

### Kullanım kolaylığı
- Program içinde aranabilir, 18 bölümlük Türkçe kullanım rehberi.
- Program içi güncelleme: yeni sürüm açılışta bulunur, tek tıkla indirilip parmak iziyle doğrulanır, yeniden başlatınca kurulur. Sürüm notları programın içinde okunur.
- Kurulum sihirbazı kurulu sürümü tanır ve güncellemeyi sorar; ayarlar ve kuyruk korunur.

<p align="center">
  <img src="https://github.com/Gakkosh/sakura-encode/raw/gorseller/ekran/rehber.png" alt="Program içi rehber" width="860">
</p>

## Kurulum

1. [Son sürüm](https://github.com/Gakkosh/sakura-encode/releases/latest) sayfasından `SakuraEncode_Setup.exe` dosyasını indir. Şu bağlantı her zaman en yeni sürümü indirir: `https://github.com/Gakkosh/sakura-encode/releases/latest/download/SakuraEncode_Setup.exe`
2. Çalıştır ve sihirbazı izle. Masaüstüne ve Başlat menüsüne kısayol eklenir.
3. Yeni sürüm çıktığında program kendisi haber verir; istersen yeni kurulum dosyasını elle de çalıştırabilirsin.

Windows "Bilgisayarınız korundu" uyarısı gösterirse **Ek bilgi** ve ardından **Yine de çalıştır**'a bas. Uygulama dijital olarak imzalı olmadığı için bu uyarı çıkabilir.

**Sistem gereksinimleri:** Windows 10 ya da 11 (64 bit). Hızlı kodlama için NVIDIA, Intel ya da AMD ekran kartı önerilir; ekran kartı olmayan bilgisayarda işlemciyle de çalışır.

## Gizlilik

Videolar, altyazılar, ayarlar ve kayıtlar yalnız bilgisayarında kalır; program hiçbirini bir yere göndermez.

Programın internete çıktığı tek yer güncelleme denetimidir: açılışta bu depodaki `info.json` dosyasından en son sürümün numarası okunur, Güncelleme sayfası açılınca sürüm notları GitHub'dan getirilir. Bu sırada hiçbir kişisel bilgi ya da dosya gönderilmez. Açılıştaki denetim Güncelleme sayfasından kapatılabilir.

## Kullanım koşulları

Sakura Encode ücretsizdir; kişisel ya da ticari işlerde kullanılabilir. Kurulum dosyası değiştirilmeden ve ücret alınmadan paylaşılabilir. Program satılamaz, değiştirilemez, kaynak koduna dönüştürülemez; "Sakura Encode" adı ve logosu izinsiz kullanılamaz. Program "olduğu gibi" sunulur, garanti verilmez. Koşulların tam metni kurulu programın klasöründeki `LICENSE.txt` dosyasındadır.

Sakura Encode, videoları [FFmpeg](https://ffmpeg.org) ile işler. FFmpeg ayrı bir program olarak kurulumla birlikte dağıtılır ve kendi lisansı (GPL) altındadır; lisans metni kurulu programın `resources/bin/FFMPEG-LICENSE.txt` dosyasındadır. Canlı önizleme videoyu [mpv](https://mpv.io) ile oynatır; mpv de ayrı bir program olarak kurulumla gelir ve kendi lisansı (GPL) altındadır, lisans metni `MPV-LICENSE.txt` adıyla onun klasöründedir. Arayüz Electron ve React (MIT lisansı) üzerine kuruludur.

© 2026 Gökhan Akkaya
