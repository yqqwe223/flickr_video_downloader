# Flickr Video İndirici 📥

> Flickr'daki herkese açık videoları cihazına kaydetmenin en basit yolu. Süs yok, takip yok, sadece çalışır.

## 👋 Bu araç neden var?

Dürüst olalım: Bazen Flickr'da gezinirken gerçekten işine yarayacak bir video buluyorsun — bir fotoğrafçılık eğitimi, bir etkinlik videosu, ya da daha önce senin yüklediğin ve şimdi yedeklemek istediğin bir içerik. Ama bunu indirip kaydetmek? Her zaman sandığın kadar kolay olmuyor.

İşte tam da bu yüzden önce kendim için yazdım bu aracı. Sonra düşündüm: "Neden başkaları da kullanmasın?". Gereksiz özellikler yok, kullanıcı takibi yok, hesap açma zorunluluğu yok. Sadece herkese açık bir Flickr linki yapıştırıyorsun, "Analiz Et"e basıyorsun, video erişilebilirse indirme seçenekleri karşına geliyor. Bu kadar.

Tüm işlemler sunucu tarafında gerçekleşiyor: Ne indirdiğini kaydetmiyorum, geçmiş tutmuyorum, kişisel veri toplamıyorum. Gizliliğin senin elinde.

## ✨ Gerçekten ne işe yarıyor?

- **Yaygın Flickr linklerini destekler**: Herkese açık albümler, kullanıcı video sayfaları, doğrudan paylaşım linkleri — özel veya şifreli olmadıkları sürece hepsiyle çalışır
- **Mevcut kaliteleri gösterir**: Flickr birden fazla çözünürlük sunuyorsa (Orijinal, Yüksek, Standart), hangisini indireceğini sen seçersin
- **Giriş yapmana gerek yok**: Sadece herkese açık içerikleri işler; Flickr kullanıcı adını veya şifreni asla istemez
- **Temiz ve responsive arayüz**: Mobil, tablet ve masaüstünde ağır frontend framework'leri olmadan düzgün görünür
- **Temel kötüye kullanım koruması**: IP başına istek limiti ile aşırı yüklenmeyi önler, hizmeti herkes için stabil tutar
- **Bloklemeyen işleme**: Uzun videolar analiz edilirken bile tarayıcı sekmen donmaz, akıcı kalır

## 🛠 Arka planda neler dönüyor?

| Katman | Kullanılan Teknolojiler |
|--------|------------------------|
| Backend | Python 3.11, Django 4.2 LTS |
| Parsing | httpx, lxml, metadata çıkarmak için regex |
| Frontend | Semantik HTML5, minimal CSS3, vanilla JS |
| Deployment | Gunicorn + Nginx, Docker uyumlu |
| Yardımcılar | python-dotenv, django-ratelimit, whitenoise |

Sıfır yapay zeka kütüphanesi. Sıfır "eve rapor gönderen" gizli API çağrısı. Sadece standart HTTP istekleri ve özenle yazılmış HTML parser'ı — okuyabileceğin, anlayabileceğin ve baş ağrıtmadan değiştirebileceğin türden kod.

## 🚀 Kendi makinenizde çalıştırmak

### Gerekenler
- Python 3.10 veya daha yeni
- pip + venv (ya da virtualenv)
- Django proje yapısı hakkında temel bilgi

### Geliştirme ortamı kurulumu

```bash
# Repoyu klonla
git clone https://github.com/kullanici-adin/flickr-downloader-tu.git
cd flickr-downloader-tu

# Sanal ortam oluştur ve aktif et
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# ya da .venv\Scripts\activate  # Windows

# Bağımlılıkları yükle
pip install -r requirements.txt

# Ortam değişkenlerini ayarla
cp .env.example .env
# .env dosyasını kendi değerlerinle düzenle (SECRET_KEY, DEBUG vb.)

# Migrationları çalıştır ve dev sunucuyu başlat
python manage.py migrate
python manage.py runserver
```

Sonra tarayıcıda `http://127.0.0.1:8000` adresini aç.

### Canlı ortam için notlar

- `DEBUG=False` yap ve `ALLOWED_HOSTS` değerini doğru belirle
- Nginx arkasında Gunicorn (veya tercih edersen uWSGI) kullan
- HTTPS'yi proxy seviyesinde etkinleştir
- Statik dosyaları topla: `python manage.py collectstatic`
- Trafik artarsa Redis ile cache eklemeyi düşün

Gunicorn örnek komutu:
```bash
gunicorn config.wsgi:application \
  --bind 127.0.0.1:8000 \
  --workers 2 \
  --timeout 90
```

Worker sayısını artırırsan bellek kullanımını takip et — video analizi biraz ağır olabilir.

## 📋 Nasıl kullanılır?

1. Video içeren herkese açık bir Flickr sayfası bul (albüm, kullanıcı profili veya paylaşılan link)
2. URL'yi kopyala ve aracın input alanına yapıştır
3. "Analiz Et" butonuna tıkla — backend mevcut video akışlarını çıkaracak
4. İşlem başarılı olursa, çözünürlük etiketli indirme butonları belirecek
5. Tercih ettiğin seçeneğe tıkla; dosya tarayıcın üzerinden inmeye başlayacak

> Not: Yalnızca herkese açık videolar çalışır. Özel albümler, sadece arkadaşlara açık içerikler, şifreli videolar veya bölgesel kısıtlamalı içerikler hata döndürür. Bu kasıtlıdır — araç Flickr'ın gizlilik ayarlarına saygı duyar.

## ⚠️ Lütfen burayı dikkatlice oku

Bu araç **yalnızca kişisel ve ticari olmayan kullanım** için tasarlanmıştır. Uygun kullanım örnekleri:
- Flickr'a senin yüklediğin videoları yedeklemek
- Herkese açık paylaşılan eğitim veya referans içeriklerini offline çalışmak üzere kaydetmek
- Fair use kapsamında araştırma veya erişilebilirlik amaçlı kullanım

**Sorumluluk sizde**:
- [Flickr Kullanım Koşulları](https://www.flickr.com/help/terms)'na uymak
- İçerik üreticilerinin telif haklarına ve Creative Commons lisanslarına saygı göstermek
- Bulunduğunuz ülkede dijital içerik kopyalamayla ilgili yürürlükteki yasalara riayet etmek

İndirmeleri izlemiyorum ve kötüye kullanım durumunda sorumluluk kabul etmiyorum. Lütfen bu aracı şu amaçlarla kullanmayın:
- Toplu veri kazıma (scraping) veya otomatik içerik toplama
- İzin alınmadan telifli materyallerin yeniden dağıtılması
- Gizlilik ayarlarını veya erişim kontrollerini aşmaya çalışmak
- Ön görüşme olmaksızın ticari hizmet veya yeniden barındırma

Kullanım durumunuzdan emin değilseniz, muhtemelen iyi değildir. Şüpheye düştüğünüzde önce içerik sahibine danışın.

## 🤝 Katkı sağlamak ister misin?

Bir hata mı buldun? Parser'ın daha sağlam olabileceğini mi düşünüyorsun? Arayüzü iyileştirecek bir fikrin mi var? Katkılar memnuniyetle karşılanır — kapı açık.

### Katkı süreci
1. Repoyu fork'la ve bir feature branch oluştur (`git checkout -b fix/mobile-layout`)
2. Değişiklikleri küçük, mantıklı commit'lerle ve açık mesajlarla kaydet
3. Lokal ortamda test et — mevcut fonksiyonların çalışmaya devam ettiğinden emin ol
4. Pull Request açarken neyin değiştiğini ve nedenini kısaça anlat

### Kod stili önerileri
- Backend: PEP 8'e uy, okunabilirliği artırdığı yerlerde type hint kullan
- Frontend: JS'yi minimal tut; ağır framework'ler yerine progressive enhancement'ı tercih et
- Commit mesajları: Konvansiyonel prefix'ler kullan (`feat:`, `fix:`, `docs:`, `chore:` vb.)

### Hata bildirimi
Bir bug raporu açarken lütfen şunları ekle:
- İlgili Flickr URL'si (paylaşılabilirse)
- Tarayıcı adı + sürümü, işletim sistemi, cihaz tipi
- Sorunu tekrarlamak için adım adım talimatlar
- Beklenen davranış ile gözlemlenen davranış arasındaki fark

Özellikle frontend sorunları için ekran görüntüsü veya konsol logları da yardımcı olur.

## 🔧 Konfigürasyon seçenekleri

| Değişken | Amaç | Örnek |
|----------|------|---------|
| `DEBUG` | Django debug modunu açıp kapatır | `False` |
| `SECRET_KEY` | Django güvenlik anahtarı | `guvenli-rastgele-string` |
| `MAX_VIDEO_SIZE_MB` | Belirtilen boyutu aşan dosyaları reddeder | `500` |
| `RATE_LIMIT_PER_MIN` | Bir IP'den dakikada gelebilecek maksimum istek | `10` |
| `ALLOWED_HOSTS` | İzin verilen alan adları (virgülle ayrılmış) | `.alanadin.com.tr` |

Tüm ayarlar `python-dotenv` üzerinden yüklenir; hiçbir gizli bilgi kaynak kodunda sabitlenmez. Production ortamında `SECRET_KEY`'inizi düzenli olarak değiştirin.

## 📄 Lisans

MIT Lisansı — tam metin için [LICENSE](./LICENSE) dosyasına bak.  
Orijinal telif hakkı bildirimini koruduğun sürece bu yazılımı özgürce kullanabilir, değiştirebilir ve dağıtabilirsin.

## 📬 İletişim & Destek

- Hata raporları ve özellik önerileri: GitHub Issues sekmesini kullan
- Genel sorular: support@twittervideodownloaderx.com
- Güvenlik açıkları: Lütfen kamuya açıklanmadan önce doğrudan e-posta ile bildir

Issue'lara genellikle birkaç gün içinde cevap vermeye çalışıyorum. Daha uzun süre geçtiyse ve yanıt alamadıysan, tekrar yazmaktan çekinme — bazen şeyler gözden kaçabiliyor.

---

*Bu proje Flickr / SmugMug, Inc. ile herhangi bir ilişki içinde değildir, tarafından onaylanmamıştır veya bağlantılı değildir. Tüm ticari markalar, logolar ve içerik hakları ilgili sahiplerine aittir.*

*Son güncelleme: Mayıs  | Sürüm 1.2.0*

*Canlı demo: https://twittervideodownloaderx.com/flickr_downloader_tu*

*Bir insan tarafından, insanlar için yazıldı. Bu README'nin veya kodun yazımında hiçbir yapay zeka kullanılmadı.*