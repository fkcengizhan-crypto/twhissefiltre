# 📊 TradingView BIST Screener — Otomatik Veri Çekici

Her iş günü saat **18:30**'da GitHub Actions otomatik olarak TradingView BIST Screener'dan tüm hisse verilerini çeker ve Excel dosyası olarak bu repoya kaydeder.

---

## 📁 Dosya Yapısı

```
├── scraper.py                          ← Ana veri çekme scripti
├── requirements.txt                    ← Python bağımlılıkları
├── .github/
│   └── workflows/
│       └── daily_scrape.yml            ← GitHub Actions zamanlama
└── data/
    ├── bist_screener_latest.xlsx       ← Her zaman en güncel dosya
    ├── bist_screener_2024-01-15.xlsx   ← Günlük arşiv
    ├── bist_screener_2024-01-16.xlsx
    └── ...
```

---

## 🚀 Kurulum (Tek Seferlik)

### 1. Bu repoyu fork'layın veya klonlayın

GitHub'da **"Use this template"** veya **Fork** butonuna tıklayın.

---

### 2. TradingView Session ID'nizi alın

Bot korumasını aşmak için TradingView hesabınızın oturum çerezine ihtiyaç var.

**Adımlar:**
1. Chrome/Firefox'ta [tradingview.com](https://tr.tradingview.com) adresine gidin
2. Hesabınıza giriş yapın
3. `F12` → **Application** (Chrome) veya **Storage** (Firefox) sekmesine tıklayın
4. Sol menüden **Cookies → https://www.tradingview.com** seçin
5. `sessionid` satırını bulun → **Value** sütunundaki değeri kopyalayın

> ⚠️ Bu değer hassas bilgidir, kimseyle paylaşmayın!

---

### 3. GitHub Secret olarak kaydedin

1. Repo sayfanızda **Settings** → **Secrets and variables** → **Actions** gidin
2. **"New repository secret"** butonuna tıklayın
3. Şu bilgileri girin:
   - **Name:** `TV_SESSION_ID`
   - **Secret:** Az önce kopyaladığınız `sessionid` değeri
4. **"Add secret"** tıklayın

---

### 4. GitHub Actions'ı Etkinleştirin

1. Repo sayfanızda **Actions** sekmesine gidin
2. "I understand my workflows, go ahead and enable them" butonuna tıklayın
3. Sol menüden **"BIST Screener - Günlük Veri Çekimi"** workflow'unu seçin
4. **"Enable workflow"** tıklayın

---

### 5. İlk çalıştırmayı test edin

1. **Actions** → **"BIST Screener - Günlük Veri Çekimi"** seçin
2. Sağ tarafta **"Run workflow"** → **"Run workflow"** tıklayın
3. ~5-10 dakika bekleyin
4. Başarılı olursa `data/` klasöründe Excel dosyası görünecek

---

## ⏰ Zamanlama

| Gün | Saat (Türkiye) | Açıklama |
|-----|---------------|----------|
| Pazartesi – Cuma | 18:30 | Borsa kapanışı sonrası |
| Hafta sonu | — | Çalışmaz |

Zamanlamayı değiştirmek için `.github/workflows/daily_scrape.yml` dosyasındaki `cron` satırını düzenleyin.

> Türkiye saatini UTC'ye çevirmek için: Türkiye saati − 3 = UTC saati  
> Örnek: 18:30 TRY → 15:30 UTC → `"30 15 * * 1-5"`

---

## 📥 Excel Dosyasına Erişim

### Yöntem 1 — GitHub'dan direkt indirin
`data/bist_screener_latest.xlsx` dosyasına tıklayın → **Download** butonuna basın.

### Yöntem 2 — Repo'yu klonlayın
```bash
git clone https://github.com/KULLANICI_ADI/REPO_ADI.git
cd REPO_ADI/data
```

### Yöntem 3 — GitHub API ile
```
https://raw.githubusercontent.com/KULLANICI_ADI/REPO_ADI/main/data/bist_screener_latest.xlsx
```

---

## 🛠️ Sorun Giderme

| Belirti | Olası Neden | Çözüm |
|---------|------------|-------|
| Workflow başarısız | Session süresi dolmuş | TV_SESSION_ID'yi güncelleyin |
| "Hiç veri çekilemedi" | TradingView yapısı değişti | Issue açın veya selector'ları güncelleyin |
| Boş Excel | Sayfa tam yüklenmedi | Workflow'u tekrar elle çalıştırın |
| Actions çalışmıyor | Uzun süre pasif repo | GitHub ~60 gün sonra zamanlamayı durdurur; Actions sekmesinden yeniden etkinleştirin |

---

## 📊 Excel İçeriği

| Sütun | İçerik |
|-------|--------|
| A | Hisse sembolü |
| B | Fiyat (TRY) |
| C–L | Performans yüzdeleri |
| M | Perf %5Y |
| N | **Ort Hacim** (Türkçe kısaltmalı: B/M/Mr) |
| O | RSI (14) |

- `bist_screener_latest.xlsx` → her zaman en güncel veriler
- `bist_screener_YYYY-MM-DD.xlsx` → günlük arşiv (tarihsel analiz için)
