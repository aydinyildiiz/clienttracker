# Fair Marble — Müşteri Takip Sistemi

Showroom'a gelen müşterileri kaydeden, gerçek zamanlı takip eden PWA (Progressive Web App).

**Canlı:** [fairmarblemusteritakipsistemi.netlify.app](https://fairmarblemusteritakipsistemi.netlify.app)

---

## Özellikler

- 🔐 Şifre koruması
- 📋 Müşteri kaydı (isim, kaynak, ürün ilgisi, satış durumu, telefon, e-posta)
- 📊 Anlık istatistikler (bugün, bu ay, satış geliri)
- 📅 Aylık rapor ekranı — kaynak dağılımı, müşteri listesi
- ☁️ Firebase Firestore ile gerçek zamanlı senkronizasyon
- 📱 PWA — iPhone/Android ana ekrana eklenebilir

---

## Teknolojiler

| Katman | Teknoloji |
|--------|-----------|
| Frontend | HTML, CSS, Vanilla JS |
| Veritabanı | Firebase Firestore |
| Hosting | Netlify |
| PWA | Web App Manifest + Service Worker |

---

## Kurulum

### 1. Firebase Projesi Oluştur

1. [console.firebase.google.com](https://console.firebase.google.com) adresine git
2. Yeni proje oluştur
3. Firestore Database → Production mode
4. Web uygulaması ekle → `firebaseConfig` değerlerini kopyala

### 2. Firebase Config Güncelle

`index.html` dosyasında şu bloğu kendi değerlerinle güncelle:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### 3. Firestore Kuralları

Firebase Console → Firestore → Rules sekmesine şunu yapıştır:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /customers/{document} {
      allow read, write: if true;
    }
  }
}
```

### 4. Şifreyi Güncelle

`index.html` içinde:

```javascript
const CORRECT_PASSWORD = 'YourPassword123';
```

### 5. Netlify'a Deploy Et

1. [app.netlify.com/drop](https://app.netlify.com/drop) adresine git
2. Tüm proje dosyalarını sürükle bırak
3. Canlı link oluşur

---

## Dosya Yapısı

```
fairmarble-tracker/
├── index.html          # Ana uygulama (tüm UI + JS)
├── manifest.json       # PWA manifest
├── service-worker.js   # Offline destek
├── icons/              # PWA ikonları (8 boyut)
│   ├── icon-72x72.png
│   ├── icon-96x96.png
│   ├── icon-128x128.png
│   ├── icon-144x144.png
│   ├── icon-152x152.png
│   ├── icon-192x192.png
│   ├── icon-384x384.png
│   └── icon-512x512.png
└── README.md
```

---

## iPhone'a Kurulum

1. Safari ile siteyi aç
2. Alt ortadaki paylaş butonuna bas
3. "Ana Ekrana Ekle" → "Ekle"
4. Ana ekranda Fair Marble ikonu ile açılır

---

## Geliştirme Notları

- Veriler Firebase Firestore'da `customers` koleksiyonunda tutulur
- Şifre doğrulandıktan sonra `localStorage`'a `fm_auth: true` yazılır
- Tüm cihazlar aynı Firebase projesini kullandığı için gerçek zamanlı senkronizasyon çalışır
- Service Worker eski versiyonları temizler, cache sorunlarını önler
