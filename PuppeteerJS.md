# 🤖 Web Scraping ve Bot Geliştirme Kapsamlı Kursu
## Puppeteer ile Profesyonel Web Otomasyonu

### 📋 İçindekiler

1. [Giriş ve Temel Kavramlar](#1-giriş-ve-temel-kavramlar)
2. [Kurulum ve İlk Adımlar](#2-kurulum-ve-i̇lk-adımlar)
3. [Temel API ve Sınıflar](#3-temel-api-ve-sınıflar)
4. [Sayfa Navigasyonu ve Etkileşim](#4-sayfa-navigasyonu-ve-etkileşim)
5. [Veri Çekme Teknikleri](#5-veri-çekme-teknikleri)
6. [Dinamik İçerik ve JavaScript](#6-dinamik-i̇çerik-ve-javascript)
7. [Form İşlemleri ve Kimlik Doğrulama](#7-form-i̇şlemleri-ve-kimlik-doğrulama)
8. [Anti-Bot Korumaları ve Çözümleri](#8-anti-bot-korumaları-ve-çözümleri)
9. [İleri Seviye Teknikler](#9-i̇leri-seviye-teknikler)
10. [Performans ve Ölçeklenebilirlik](#10-performans-ve-ölçeklenebilirlik)
11. [Test Otomasyonu](#11-test-otomasyonu)
12. [En İyi Uygulamalar](#12-en-i̇yi-uygulamalar)
13. [Veri Saklama ve Veritabanı Entegrasyonu](#13-veri-saklama-ve-veritabanı-entegrasyonu)
14. [Environment ve Konfigürasyon Yönetimi](#14-environment-ve-konfigürasyon-yönetimi)
15. [Veri Doğrulama ve Temizleme](#15-veri-doğrulama-ve-temizleme)
16. [Popüler Site Desenleri](#16-popüler-site-desenleri)
17. [Webhook ve Bildirimler](#17-webhook-ve-bildirimler)
18. [Docker ve Containerization](#18-docker-ve-containerization)
19. [CI/CD ve Production Deployment](#19-cicd-ve-production-deployment)

---

## 1. Giriş ve Temel Kavramlar

### Web Scraping Nedir?

Web scraping, web sitelerinden otomatik olarak veri çekme işlemidir. Puppeteer, Google Chrome'u kontrol etmek için kullanılan Node.js kütüphanesidir ve headless (görsel arayüz olmadan) çalışır.

### Puppeteer'ın Avantajları

- **JavaScript Desteği**: SPA (Single Page Application) ve dinamik içerik
- **Gerçek Tarayıcı**: Chrome DevTools Protocol kullanır
- **Güçlü API**: Tüm tarayıcı özelliklerine erişim
- **Screenshot ve PDF**: Sayfa görüntüleri ve doküman oluşturma
- **Network İnterception**: İstek/yanıt manipülasyonu

### Kullanım Alanları

- E-ticaret fiyat takibi
- Sosyal medya analizi
- İçerik toplama ve arşivleme
- Otomatik test senaryoları
- SEO ve site analizi

---

## 2. Kurulum ve İlk Adımlar

### Sistem Gereksinimleri

```bash
# Node.js (v16.3.0 veya üstü)
node --version

# npm veya yarn
npm --version
```

### Puppeteer Kurulumu

```bash
# Temel kurulum
npm install puppeteer

# Hafif versiyon (Chrome indirmez)
npm install puppeteer-core

# Geliştirme bağımlılıkları
npm install --save-dev puppeteer
```

### İlk Örnek

```javascript
import puppeteer from 'puppeteer';

(async () => {
  // Tarayıcı başlat
  const browser = await puppeteer.launch({
    headless: false, // Görsel mod
    devtools: true   // DevTools aç
  });
  
  // Yeni sayfa oluştur
  const page = await browser.newPage();
  
  // Web sitesine git
  await page.goto('https://example.com');
  
  // Başlık al
  const title = await page.title();
  console.log('Sayfa Başlığı:', title);
  
  // Tarayıcıyı kapat
  await browser.close();
})();
```

### Proje Yapısı

```
proje/
├── src/
│   ├── scrapers/     # Scraper sınıfları
│   ├── utils/        # Yardımcı fonksiyonlar
│   └── config/       # Yapılandırma dosyaları
├── data/             # Çekilen veriler
├── logs/             # Log dosyaları
└── package.json
```

---

## 3. Temel API ve Sınıflar

### Browser Sınıfı

```javascript
const browser = await puppeteer.launch({
  headless: 'new',           // Yeni headless modu
  args: [
    '--no-sandbox',
    '--disable-setuid-sandbox',
    '--disable-web-security',
    '--disable-dev-shm-usage'
  ],
  executablePath: '/path/to/chrome', // Özel Chrome yolu
  defaultViewport: {
    width: 1920,
    height: 1080
  },
  timeout: 30000,
  slowMo: 100               // İşlemler arası gecikme
});

// Tarayıcı bilgileri
console.log(await browser.version());
console.log(browser.wsEndpoint());

// Tüm sayfaları listele
const pages = await browser.pages();

// Yeni context oluştur
const context = await browser.createBrowserContext();
```

### Page Sınıfı

```javascript
const page = await browser.newPage();

// Sayfa ayarları
await page.setViewport({ width: 1280, height: 720 });
await page.setUserAgent('Mozilla/5.0 Custom Bot');

// Extra HTTP headers
await page.setExtraHTTPHeaders({
  'Accept-Language': 'tr-TR,tr;q=0.9,en;q=0.8'
});

// JavaScript etkinleştir/devre dışı bırak
await page.setJavaScriptEnabled(false);

// Geolocation ayarla
await page.setGeolocation({ latitude: 41.0082, longitude: 28.9784 });

// Offline modu
await page.setOfflineMode(true);

// CPU throttling
const client = await page.target().createCDPSession();
await client.send('Emulation.setCPUThrottlingRate', { rate: 2 });
```

### ElementHandle ile Etkileşim

```javascript
// Element seçme
const element = await page.$('button#submit');
const elements = await page.$$('div.product');

// Element özellikleri
const text = await element.innerText();
const html = await element.innerHTML();
const value = await element.inputValue();
const isVisible = await element.isVisible();
const isEnabled = await element.isEnabled();

// Element eylemleri
await element.click();
await element.type('Merhaba dünya');
await element.press('Enter');
await element.hover();
await element.focus();
await element.selectOption('option1');
```

---

## 4. Sayfa Navigasyonu ve Etkileşim

### Navigasyon İşlemleri

```javascript
// Temel navigasyon
await page.goto('https://example.com', {
  waitUntil: 'networkidle2',  // Ağ boş kalana kadar bekle
  timeout: 30000
});

// Sayfa geçmişi
await page.goBack();
await page.goForward();
await page.reload();

// Yönlendirme kontrolü
const response = await page.goto('https://example.com');
console.log('Durum Kodu:', response.status());
console.log('URL:', response.url());

// Çoklu navigasyon
await Promise.all([
  page.waitForNavigation(),
  page.click('a[href="/next-page"]')
]);
```

### Bekleme Stratejileri

```javascript
// Element bekle
await page.waitForSelector('div.content', {
  visible: true,
  timeout: 10000
});

// Fonksiyon bekle
await page.waitForFunction(() => {
  return document.querySelector('div.loaded');
}, {}, { timeout: 15000 });

// Ağ isteği bekle
await page.waitForResponse(response => 
  response.url().includes('/api/data') && response.status() === 200
);

// Manuel gecikme
await page.waitForTimeout(2000);

// Sayfa yükleme durumu
await page.waitForLoadState('networkidle');
```

### Mouse ve Klavye İşlemleri

```javascript
// Mouse işlemleri
await page.mouse.move(100, 200);
await page.mouse.click(100, 200);
await page.mouse.down();
await page.mouse.up();
await page.mouse.wheel({ deltaY: -100 });

// Klavye işlemleri
await page.keyboard.type('Aranacak kelime');
await page.keyboard.press('Enter');
await page.keyboard.down('Shift');
await page.keyboard.up('Shift');

// Kısayollar
await page.keyboard.press('Control+A');
await page.keyboard.press('Control+C');
await page.keyboard.press('Control+V');
```

---

## 5. Veri Çekme Teknikleri

### Temel Veri Çekme

```javascript
// Tek element
const title = await page.$eval('h1', el => el.textContent);

// Çoklu elementler
const links = await page.$$eval('a', anchors => 
  anchors.map(anchor => ({
    text: anchor.textContent,
    href: anchor.href
  }))
);

// Sayfa evaluate
const pageData = await page.evaluate(() => {
  return {
    title: document.title,
    url: window.location.href,
    cookies: document.cookie,
    localStorage: JSON.stringify(localStorage),
    customData: window.myCustomData
  };
});
```

### İleri Seviye Veri Çekme

```javascript
class ProductScraper {
  constructor(page) {
    this.page = page;
  }
  
  async scrapeProduct(selector) {
    return await this.page.evaluate((sel) => {
      const product = document.querySelector(sel);
      if (!product) return null;
      
      return {
        title: product.querySelector('.title')?.textContent?.trim(),
        price: product.querySelector('.price')?.textContent?.trim(),
        image: product.querySelector('img')?.src,
        rating: product.querySelector('.rating')?.textContent?.trim(),
        availability: product.querySelector('.stock')?.textContent?.includes('Stokta'),
        description: product.querySelector('.description')?.textContent?.trim(),
        features: Array.from(product.querySelectorAll('.features li')).map(li => li.textContent.trim())
      };
    }, selector);
  }
  
  async scrapeAllProducts() {
    const productSelectors = await this.page.$$eval('.product-item', 
      items => items.map((_, index) => `.product-item:nth-child(${index + 1})`)
    );
    
    const products = [];
    for (const selector of productSelectors) {
      const product = await this.scrapeProduct(selector);
      if (product) products.push(product);
    }
    
    return products;
  }
}

// Kullanım
const scraper = new ProductScraper(page);
const products = await scraper.scrapeAllProducts();
```

### CSV ve JSON Export

```javascript
import fs from 'fs';
import csv from 'csv-writer';

class DataExporter {
  static async saveAsJSON(data, filename) {
    const json = JSON.stringify(data, null, 2);
    await fs.promises.writeFile(`data/${filename}.json`, json);
  }
  
  static async saveAsCSV(data, filename, headers) {
    const csvWriter = csv.createObjectCsvWriter({
      path: `data/${filename}.csv`,
      header: headers
    });
    
    await csvWriter.writeRecords(data);
  }
  
  static async saveWithTimestamp(data, prefix) {
    const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
    const filename = `${prefix}-${timestamp}`;
    
    await Promise.all([
      this.saveAsJSON(data, filename),
      this.saveAsCSV(data, filename, Object.keys(data[0] || {}))
    ]);
  }
}
```

---

## 6. Dinamik İçerik ve JavaScript

### SPA (Single Page Application) Scraping

```javascript
class SPAScraper {
  constructor(page) {
    this.page = page;
  }
  
  async waitForDynamicContent(selector, timeout = 30000) {
    try {
      await this.page.waitForSelector(selector, { 
        visible: true, 
        timeout 
      });
      
      // İçerik değişimini bekle
      await this.page.waitForFunction((sel) => {
        const element = document.querySelector(sel);
        return element && element.textContent.trim() !== '';
      }, {}, selector);
      
      return true;
    } catch (error) {
      console.log(`Timeout: ${selector} bulunamadı`);
      return false;
    }
  }
  
  async handleInfiniteScroll() {
    let previousHeight = 0;
    let currentHeight = await this.page.evaluate('document.body.scrollHeight');
    
    while (previousHeight !== currentHeight) {
      previousHeight = currentHeight;
      
      // Sayfanın sonuna scroll
      await this.page.evaluate('window.scrollTo(0, document.body.scrollHeight)');
      
      // Yeni içeriğin yüklenmesini bekle
      await this.page.waitForTimeout(2000);
      
      currentHeight = await this.page.evaluate('document.body.scrollHeight');
    }
  }
  
  async clickLoadMore() {
    while (true) {
      try {
        const loadMoreBtn = await this.page.$('button[data-testid="load-more"]');
        if (!loadMoreBtn) break;
        
        const isVisible = await loadMoreBtn.isVisible();
        if (!isVisible) break;
        
        await loadMoreBtn.click();
        await this.page.waitForTimeout(2000);
        
      } catch (error) {
        console.log('Load More butonu bulunamadı, scraping tamamlandı');
        break;
      }
    }
  }
}
```

### AJAX İsteklerini İzleme

```javascript
class AjaxTracker {
  constructor(page) {
    this.page = page;
    this.requests = [];
    this.responses = [];
    
    this.setupListeners();
  }
  
  setupListeners() {
    this.page.on('request', request => {
      if (request.resourceType() === 'xhr' || request.resourceType() === 'fetch') {
        this.requests.push({
          url: request.url(),
          method: request.method(),
          headers: request.headers(),
          postData: request.postData(),
          timestamp: Date.now()
        });
      }
    });
    
    this.page.on('response', response => {
      if (response.request().resourceType() === 'xhr' || 
          response.request().resourceType() === 'fetch') {
        this.responses.push({
          url: response.url(),
          status: response.status(),
          headers: response.headers(),
          timestamp: Date.now()
        });
      }
    });
  }
  
  async waitForSpecificAPI(urlPattern, timeout = 15000) {
    return new Promise((resolve, reject) => {
      const timer = setTimeout(() => {
        reject(new Error(`API isteği timeout: ${urlPattern}`));
      }, timeout);
      
      const checkResponse = () => {
        const found = this.responses.find(resp => 
          resp.url.includes(urlPattern) && resp.status === 200
        );
        
        if (found) {
          clearTimeout(timer);
          resolve(found);
        } else {
          setTimeout(checkResponse, 100);
        }
      };
      
      checkResponse();
    });
  }
  
  async getAPIData(urlPattern) {
    const response = await this.page.waitForResponse(
      response => response.url().includes(urlPattern) && response.status() === 200
    );
    
    return await response.json();
  }
}
```

---

## 7. Form İşlemleri ve Kimlik Doğrulama

### Form Doldurma ve Gönderme

```javascript
class FormHandler {
  constructor(page) {
    this.page = page;
  }
  
  async fillForm(formSelector, data) {
    for (const [field, value] of Object.entries(data)) {
      const selector = `${formSelector} [name="${field}"], ${formSelector} #${field}`;
      
      try {
        const element = await this.page.$(selector);
        if (!element) continue;
        
        const tagName = await element.evaluate(el => el.tagName.toLowerCase());
        const type = await element.evaluate(el => el.type || '');
        
        switch (tagName) {
          case 'input':
            if (type === 'checkbox' || type === 'radio') {
              if (value) await element.click();
            } else {
              await element.clear();
              await element.type(value.toString());
            }
            break;
            
          case 'select':
            await element.selectOption(value.toString());
            break;
            
          case 'textarea':
            await element.clear();
            await element.type(value.toString());
            break;
        }
        
      } catch (error) {
        console.log(`Form alanı doldurulamadı: ${field}`);
      }
    }
  }
  
  async submitForm(formSelector, submitSelector = 'button[type="submit"]') {
    await Promise.all([
      this.page.waitForNavigation({ waitUntil: 'networkidle2' }),
      this.page.click(`${formSelector} ${submitSelector}`)
    ]);
  }
  
  async handleCaptcha() {
    // reCAPTCHA algılama
    const recaptcha = await this.page.$('div.g-recaptcha');
    if (recaptcha) {
      console.log('reCAPTCHA algılandı. Manuel çözüm bekleniyor...');
      await this.page.waitForTimeout(30000); // 30 saniye bekle
    }
    
    // hCaptcha algılama
    const hcaptcha = await this.page.$('div.h-captcha');
    if (hcaptcha) {
      console.log('hCaptcha algılandı. Manuel çözüm bekleniyor...');
      await this.page.waitForTimeout(30000);
    }
  }
}
```

### Giriş Yapma ve Session Yönetimi

```javascript
class AuthManager {
  constructor(page) {
    this.page = page;
    this.cookieFile = 'cookies.json';
  }
  
  async login(credentials) {
    await this.page.goto('https://example.com/login');
    
    // Giriş formu doldur
    await this.page.type('#email', credentials.email);
    await this.page.type('#password', credentials.password);
    
    // reCAPTCHA kontrolü
    const formHandler = new FormHandler(this.page);
    await formHandler.handleCaptcha();
    
    // Giriş yap ve navigasyon bekle
    await Promise.all([
      this.page.waitForNavigation({ waitUntil: 'networkidle2' }),
      this.page.click('button[type="submit"]')
    ]);
    
    // Giriş başarılı mı kontrol et
    const isLoggedIn = await this.page.$('.user-dashboard') !== null;
    
    if (isLoggedIn) {
      await this.saveCookies();
      return true;
    }
    
    return false;
  }
  
  async saveCookies() {
    const cookies = await this.page.cookies();
    await fs.promises.writeFile(this.cookieFile, JSON.stringify(cookies, null, 2));
  }
  
  async loadCookies() {
    try {
      const cookieData = await fs.promises.readFile(this.cookieFile, 'utf8');
      const cookies = JSON.parse(cookieData);
      await this.page.setCookie(...cookies);
      return true;
    } catch (error) {
      console.log('Cookie dosyası bulunamadı');
      return false;
    }
  }
  
  async checkLoginStatus() {
    await this.page.goto('https://example.com/dashboard');
    
    // Giriş sayfasına yönlendirildi mi kontrol et
    const currentUrl = this.page.url();
    return !currentUrl.includes('/login');
  }
  
  async autoLogin(credentials) {
    // Önce kaydedilmiş cookie'leri dene
    const cookiesLoaded = await this.loadCookies();
    
    if (cookiesLoaded) {
      const isLoggedIn = await this.checkLoginStatus();
      if (isLoggedIn) {
        console.log('Cookie ile giriş başarılı');
        return true;
      }
    }
    
    // Cookie ile giriş başarısız, manuel giriş yap
    console.log('Manuel giriş yapılıyor...');
    return await this.login(credentials);
  }
}
```

---

## 8. Anti-Bot Korumaları ve Çözümleri

### Temel Stealth Teknikleri

```javascript
import StealthPlugin from 'puppeteer-extra-plugin-stealth';
import puppeteer from 'puppeteer-extra';

// Stealth plugin ekle
puppeteer.use(StealthPlugin());

class StealthScraper {
  static async launch() {
    const browser = await puppeteer.launch({
      headless: 'new',
      args: [
        '--no-sandbox',
        '--disable-setuid-sandbox',
        '--disable-blink-features=AutomationControlled',
        '--disable-extensions-except=/path/to/extension',
        '--disable-extensions',
        '--disable-default-apps',
        '--disable-component-extensions-with-background-pages'
      ]
    });
    
    return browser;
  }
  
  static async setupStealthPage(page) {
    // User agent randomize
    const userAgents = [
      'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36',
      'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36',
      'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36'
    ];
    
    const randomUA = userAgents[Math.floor(Math.random() * userAgents.length)];
    await page.setUserAgent(randomUA);
    
    // Viewport randomize
    const viewports = [
      { width: 1920, height: 1080 },
      { width: 1366, height: 768 },
      { width: 1440, height: 900 },
      { width: 1536, height: 864 }
    ];
    
    const randomVP = viewports[Math.floor(Math.random() * viewports.length)];
    await page.setViewport(randomVP);
    
    // WebDriver özelliklerini gizle
    await page.evaluateOnNewDocument(() => {
      Object.defineProperty(navigator, 'webdriver', {
        get: () => false,
      });
    });
    
    // Chrome detection bypass
    await page.evaluateOnNewDocument(() => {
      window.chrome = {
        runtime: {},
        // Chrome objesi mock
      };
    });
    
    // Plugin bilgilerini sahte ayarla
    await page.evaluateOnNewDocument(() => {
      Object.defineProperty(navigator, 'plugins', {
        get: () => [
          {
            0: { type: "application/x-google-chrome-pdf", suffixes: "pdf", description: "Portable Document Format" },
            description: "Portable Document Format",
            filename: "internal-pdf-viewer",
            length: 1,
            name: "Chrome PDF Plugin"
          }
        ],
      });
    });
    
    return page;
  }
}
```

### Rate Limiting ve Proxy Yönetimi

```javascript
class RateLimiter {
  constructor(maxRequests = 10, timeWindow = 60000) {
    this.maxRequests = maxRequests;
    this.timeWindow = timeWindow;
    this.requests = [];
  }
  
  async waitIfNeeded() {
    const now = Date.now();
    
    // Eski istekleri temizle
    this.requests = this.requests.filter(time => now - time < this.timeWindow);
    
    // Limit aşıldı mı kontrol et
    if (this.requests.length >= this.maxRequests) {
      const oldestRequest = Math.min(...this.requests);
      const waitTime = this.timeWindow - (now - oldestRequest);
      
      if (waitTime > 0) {
        console.log(`Rate limit aşıldı. ${waitTime}ms bekleniyor...`);
        await new Promise(resolve => setTimeout(resolve, waitTime));
      }
    }
    
    // Yeni isteği kaydet
    this.requests.push(now);
  }
  
  static randomDelay(min = 1000, max = 5000) {
    const delay = Math.floor(Math.random() * (max - min + 1)) + min;
    return new Promise(resolve => setTimeout(resolve, delay));
  }
}

class ProxyManager {
  constructor(proxies = []) {
    this.proxies = proxies;
    this.currentIndex = 0;
  }
  
  getNext() {
    if (this.proxies.length === 0) return null;
    
    const proxy = this.proxies[this.currentIndex];
    this.currentIndex = (this.currentIndex + 1) % this.proxies.length;
    return proxy;
  }
  
  async launchWithProxy() {
    const proxy = this.getNext();
    
    if (!proxy) {
      return await puppeteer.launch();
    }
    
    const browser = await puppeteer.launch({
      args: [
        `--proxy-server=${proxy.host}:${proxy.port}`,
        '--no-sandbox',
        '--disable-setuid-sandbox'
      ]
    });
    
    // Proxy auth gerekirse
    if (proxy.username && proxy.password) {
      const page = await browser.newPage();
      await page.authenticate({
        username: proxy.username,
        password: proxy.password
      });
    }
    
    return browser;
  }
}
```

### Cloudflare Bypass

```javascript
class CloudflareBypass {
  constructor(page) {
    this.page = page;
  }
  
  async detectCloudflare() {
    const selectors = [
      'div.cf-browser-verification',
      'div#cf-wrapper',
      'div.cf-im-under-attack'
    ];
    
    for (const selector of selectors) {
      const element = await this.page.$(selector);
      if (element) {
        console.log('Cloudflare koruması algılandı');
        return true;
      }
    }
    
    return false;
  }
  
  async waitForCloudflareBypass(timeout = 30000) {
    console.log('Cloudflare bypass bekleniyor...');
    
    const startTime = Date.now();
    
    while (Date.now() - startTime < timeout) {
      const isCloudflare = await this.detectCloudflare();
      
      if (!isCloudflare) {
        console.log('Cloudflare bypass başarılı');
        return true;
      }
      
      await this.page.waitForTimeout(1000);
    }
    
    throw new Error('Cloudflare bypass timeout');
  }
  
  async handleChallenge() {
    // Challenge detection
    const challengeForm = await this.page.$('form#challenge-form');
    
    if (challengeForm) {
      console.log('Cloudflare challenge algılandı');
      
      // Manuel çözüm bekle veya otomatik çöz
      await this.page.waitForSelector('form#challenge-form', {
        hidden: true,
        timeout: 30000
      });
      
      console.log('Challenge çözüldü');
    }
  }
}
```

---

## 9. İleri Seviye Teknikler

### Network İsteklerini İnterceptleme

```javascript
class NetworkInterceptor {
  constructor(page) {
    this.page = page;
    this.interceptedRequests = new Map();
    this.mockResponses = new Map();
  }
  
  async enable() {
    await this.page.setRequestInterception(true);
    
    this.page.on('request', request => this.handleRequest(request));
    this.page.on('response', response => this.handleResponse(response));
  }
  
  async handleRequest(request) {
    const url = request.url();
    
    // İstek logla
    console.log(`İstek: ${request.method()} ${url}`);
    
    // Belirli kaynak türlerini blokla
    if (request.resourceType() === 'image' || 
        request.resourceType() === 'stylesheet' ||
        request.resourceType() === 'font') {
      await request.abort();
      return;
    }
    
    // Mock response varsa kullan
    if (this.mockResponses.has(url)) {
      const mockResponse = this.mockResponses.get(url);
      await request.respond(mockResponse);
      return;
    }
    
    // Headers değiştir
    const headers = {
      ...request.headers(),
      'X-Custom-Header': 'puppeteer-bot'
    };
    
    await request.continue({ headers });
  }
  
  async handleResponse(response) {
    const url = response.url();
    const status = response.status();
    
    console.log(`Yanıt: ${status} ${url}`);
    
    // API yanıtlarını kaydet
    if (url.includes('/api/')) {
      try {
        const data = await response.json();
        this.interceptedRequests.set(url, data);
      } catch (error) {
        // JSON değilse text olarak kaydet
        const text = await response.text();
        this.interceptedRequests.set(url, text);
      }
    }
  }
  
  setMockResponse(url, response) {
    this.mockResponses.set(url, response);
  }
  
  getInterceptedData(url) {
    return this.interceptedRequests.get(url);
  }
}
```

### Screenshot ve PDF Oluşturma

```javascript
class DocumentGenerator {
  constructor(page) {
    this.page = page;
  }
  
  async takeScreenshot(options = {}) {
    const defaultOptions = {
      path: `screenshot-${Date.now()}.png`,
      fullPage: true,
      type: 'png',
      quality: 90
    };
    
    const finalOptions = { ...defaultOptions, ...options };
    
    // Sayfanın tamamen yüklenmesini bekle
    await this.page.waitForLoadState('networkidle');
    
    return await this.page.screenshot(finalOptions);
  }
  
  async takeElementScreenshot(selector, options = {}) {
    const element = await this.page.$(selector);
    
    if (!element) {
      throw new Error(`Element bulunamadı: ${selector}`);
    }
    
    return await element.screenshot({
      path: `element-${Date.now()}.png`,
      type: 'png',
      ...options
    });
  }
  
  async generatePDF(options = {}) {
    const defaultOptions = {
      path: `document-${Date.now()}.pdf`,
      format: 'A4',
      printBackground: true,
      margin: {
        top: '1cm',
        right: '1cm',
        bottom: '1cm',
        left: '1cm'
      }
    };
    
    const finalOptions = { ...defaultOptions, ...options };
    
    return await this.page.pdf(finalOptions);
  }
  
  async capturePageMetrics() {
    const metrics = await this.page.metrics();
    
    return {
      ...metrics,
      timestamp: new Date().toISOString(),
      url: this.page.url(),
      title: await this.page.title()
    };
  }
}
```

### Paralel İşlem Yönetimi

```javascript
class ParallelScraper {
  constructor(concurrency = 5) {
    this.concurrency = concurrency;
    this.browser = null;
    this.semaphore = new Semaphore(concurrency);
  }
  
  async initialize() {
    this.browser = await puppeteer.launch({
      headless: 'new',
      args: ['--no-sandbox', '--disable-dev-shm-usage']
    });
  }
  
  async scrapeURLs(urls, scraperFunction) {
    if (!this.browser) {
      await this.initialize();
    }
    
    const results = await Promise.allSettled(
      urls.map(url => this.scrapeSingle(url, scraperFunction))
    );
    
    return results.map((result, index) => ({
      url: urls[index],
      success: result.status === 'fulfilled',
      data: result.status === 'fulfilled' ? result.value : null,
      error: result.status === 'rejected' ? result.reason : null
    }));
  }
  
  async scrapeSingle(url, scraperFunction) {
    await this.semaphore.acquire();
    
    let page = null;
    
    try {
      page = await this.browser.newPage();
      await StealthScraper.setupStealthPage(page);
      
      await page.goto(url, { 
        waitUntil: 'networkidle2',
        timeout: 30000 
      });
      
      const result = await scraperFunction(page, url);
      
      return result;
      
    } catch (error) {
      console.error(`Scraping hatası - ${url}:`, error.message);
      throw error;
      
    } finally {
      if (page) {
        await page.close();
      }
      this.semaphore.release();
    }
  }
  
  async close() {
    if (this.browser) {
      await this.browser.close();
    }
  }
}

class Semaphore {
  constructor(count) {
    this.count = count;
    this.waiting = [];
  }
  
  async acquire() {
    return new Promise((resolve) => {
      if (this.count > 0) {
        this.count--;
        resolve();
      } else {
        this.waiting.push(resolve);
      }
    });
  }
  
  release() {
    if (this.waiting.length > 0) {
      const resolve = this.waiting.shift();
      resolve();
    } else {
      this.count++;
    }
  }
}
```

---

## 10. Performans ve Ölçeklenebilirlik

### Kaynak Optimizasyonu

```javascript
class PerformanceOptimizer {
  static async createOptimizedPage(browser) {
    const page = await browser.newPage();
    
    // Gereksiz kaynak türlerini blokla
    await page.setRequestInterception(true);
    
    page.on('request', (request) => {
      const resourceType = request.resourceType();
      
      if (['image', 'stylesheet', 'font', 'media'].includes(resourceType)) {
        request.abort();
      } else {
        request.continue();
      }
    });
    
    // CPU ve memory optimizasyonu
    const client = await page.target().createCDPSession();
    
    await client.send('Emulation.setCPUThrottlingRate', { rate: 1 });
    await client.send('Runtime.enable');
    
    // Sayfa ayarları
    await page.setViewport({ 
      width: 1280, 
      height: 720,
      deviceScaleFactor: 1 
    });
    
    await page.setJavaScriptEnabled(true);
    await page.setCacheEnabled(false);
    
    return page;
  }
  
  static async measurePageLoad(page, url) {
    const startTime = Date.now();
    
    const response = await page.goto(url, {
      waitUntil: 'networkidle2',
      timeout: 30000
    });
    
    const endTime = Date.now();
    const loadTime = endTime - startTime;
    
    const metrics = await page.metrics();
    
    return {
      url,
      loadTime,
      statusCode: response.status(),
      metrics: {
        jsHeapUsedSize: metrics.JSHeapUsedSize,
        jsHeapTotalSize: metrics.JSHeapTotalSize,
        scriptDuration: metrics.ScriptDuration,
        taskDuration: metrics.TaskDuration
      }
    };
  }
}
```

### Queue Sistemi

```javascript
import Queue from 'bull';
import Redis from 'ioredis';

class ScrapingQueue {
  constructor(redisConfig = {}) {
    this.redis = new Redis(redisConfig);
    this.queue = new Queue('scraping jobs', {
      redis: redisConfig
    });
    
    this.setupProcessors();
  }
  
  setupProcessors() {
    this.queue.process('scrape-url', 5, async (job) => {
      const { url, options } = job.data;
      
      console.log(`İşleniyor: ${url}`);
      
      const browser = await puppeteer.launch({
        headless: 'new',
        args: ['--no-sandbox']
      });
      
      try {
        const page = await PerformanceOptimizer.createOptimizedPage(browser);
        await page.goto(url);
        
        // Scraping logic
        const data = await page.evaluate(() => {
          return {
            title: document.title,
            text: document.body.innerText,
            links: Array.from(document.links).map(l => l.href)
          };
        });
        
        // Sonucu Redis'e kaydet
        await this.redis.set(`result:${job.id}`, JSON.stringify(data));
        
        return data;
        
      } finally {
        await browser.close();
      }
    });
    
    // Event listeners
    this.queue.on('completed', (job, result) => {
      console.log(`Tamamlandı: ${job.id}`);
    });
    
    this.queue.on('failed', (job, err) => {
      console.log(`Başarısız: ${job.id} - ${err.message}`);
    });
  }
  
  async addURL(url, options = {}, priority = 0) {
    const job = await this.queue.add('scrape-url', 
      { url, options }, 
      { 
        priority,
        attempts: 3,
        backoff: 'exponential',
        delay: 1000
      }
    );
    
    return job.id;
  }
  
  async getResult(jobId) {
    const result = await this.redis.get(`result:${jobId}`);
    return result ? JSON.parse(result) : null;
  }
  
  async getStats() {
    return {
      waiting: await this.queue.getWaiting().then(jobs => jobs.length),
      active: await this.queue.getActive().then(jobs => jobs.length),
      completed: await this.queue.getCompleted().then(jobs => jobs.length),
      failed: await this.queue.getFailed().then(jobs => jobs.length)
    };
  }
}
```

### Monitoring ve Loglama

```javascript
import winston from 'winston';

class ScrapingMonitor {
  constructor() {
    this.logger = winston.createLogger({
      level: 'info',
      format: winston.format.combine(
        winston.format.timestamp(),
        winston.format.json()
      ),
      transports: [
        new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
        new winston.transports.File({ filename: 'logs/scraping.log' }),
        new winston.transports.Console({
          format: winston.format.simple()
        })
      ]
    });
    
    this.stats = {
      totalRequests: 0,
      successfulRequests: 0,
      failedRequests: 0,
      startTime: Date.now()
    };
  }
  
  logRequest(url, method = 'GET') {
    this.stats.totalRequests++;
    this.logger.info('Request started', { url, method });
  }
  
  logSuccess(url, duration, dataSize = 0) {
    this.stats.successfulRequests++;
    this.logger.info('Request successful', {
      url,
      duration,
      dataSize,
      success: true
    });
  }
  
  logError(url, error, duration) {
    this.stats.failedRequests++;
    this.logger.error('Request failed', {
      url,
      error: error.message,
      duration,
      success: false
    });
  }
  
  getStats() {
    const uptime = Date.now() - this.stats.startTime;
    const successRate = (this.stats.successfulRequests / this.stats.totalRequests) * 100;
    
    return {
      ...this.stats,
      uptime,
      successRate: successRate.toFixed(2) + '%',
      requestsPerMinute: ((this.stats.totalRequests / uptime) * 60000).toFixed(2)
    };
  }
}
```

---

## 11. Test Otomasyonu

### E2E Test Senaryoları

```javascript
import { describe, it, beforeAll, afterAll } from '@jest/globals';

describe('E-ticaret Site Testleri', () => {
  let browser;
  let page;
  
  beforeAll(async () => {
    browser = await puppeteer.launch({
      headless: 'new',
      slowMo: 100
    });
    page = await browser.newPage();
  });
  
  afterAll(async () => {
    await browser.close();
  });
  
  it('Ana sayfa yüklenmeli', async () => {
    await page.goto('https://example-shop.com');
    
    const title = await page.title();
    expect(title).toContain('Online Mağaza');
    
    // Logo kontrolü
    const logo = await page.$('.logo');
    expect(logo).toBeTruthy();
    
    // Menü kontrolü
    const menu = await page.$('nav.main-menu');
    expect(menu).toBeTruthy();
  });
  
  it('Ürün arama işlevi çalışmalı', async () => {
    await page.type('.search-input', 'laptop');
    await page.click('.search-button');
    
    await page.waitForSelector('.product-list');
    
    const products = await page.$$('.product-item');
    expect(products.length).toBeGreaterThan(0);
    
    const firstProductTitle = await page.$eval('.product-item:first-child .title', 
      el => el.textContent
    );
    expect(firstProductTitle.toLowerCase()).toContain('laptop');
  });
  
  it('Sepete ürün ekleme', async () => {
    // İlk ürüne tıkla
    await page.click('.product-item:first-child');
    await page.waitForSelector('.product-detail');
    
    // Sepete ekle
    await page.click('.add-to-cart');
    
    // Sepet ikonunda sayı kontrolü
    await page.waitForSelector('.cart-count');
    const cartCount = await page.$eval('.cart-count', el => el.textContent);
    expect(parseInt(cartCount)).toBe(1);
  });
  
  it('Checkout süreci', async () => {
    // Sepete git
    await page.click('.cart-icon');
    await page.waitForSelector('.cart-page');
    
    // Checkout başlat
    await page.click('.checkout-button');
    await page.waitForSelector('.checkout-form');
    
    // Form doldur
    const formData = {
      firstName: 'John',
      lastName: 'Doe',
      email: 'john@example.com',
      phone: '555-0123',
      address: '123 Ana Cadde',
      city: 'İstanbul',
      postalCode: '34000'
    };
    
    for (const [field, value] of Object.entries(formData)) {
      await page.type(`[name="${field}"]`, value);
    }
    
    // Ödeme bilgileri
    await page.type('[name="cardNumber"]', '4111111111111111');
    await page.type('[name="expiryDate"]', '12/25');
    await page.type('[name="cvv"]', '123');
    
    // Siparişi tamamla
    await page.click('.complete-order');
    
    // Başarı sayfası kontrolü
    await page.waitForSelector('.order-success');
    const successMessage = await page.$eval('.order-success h1', 
      el => el.textContent
    );
    expect(successMessage).toContain('Siparişiniz alındı');
  });
});
```

### Performance Testing

```javascript
class PerformanceTester {
  constructor(page) {
    this.page = page;
    this.metrics = [];
  }
  
  async measurePagePerformance(url) {
    // Performance timing başlat
    await this.page.goto(url, { waitUntil: 'networkidle2' });
    
    const metrics = await this.page.evaluate(() => {
      const timing = performance.timing;
      const navigation = performance.getEntriesByType('navigation')[0];
      
      return {
        // Temel timing metrikleri
        dns: timing.domainLookupEnd - timing.domainLookupStart,
        tcp: timing.connectEnd - timing.connectStart,
        ttfb: timing.responseStart - timing.requestStart,
        domLoading: timing.domContentLoadedEventEnd - timing.navigationStart,
        domComplete: timing.domComplete - timing.navigationStart,
        loadComplete: timing.loadEventEnd - timing.navigationStart,
        
        // Navigation API metrikleri
        domInteractive: navigation.domInteractive,
        domContentLoaded: navigation.domContentLoadedEventEnd,
        loadEventEnd: navigation.loadEventEnd,
        
        // Core Web Vitals
        fcp: performance.getEntriesByName('first-contentful-paint')[0]?.startTime || 0,
        lcp: 0, // Largest Contentful Paint (ayrıca hesaplanacak)
        cls: 0, // Cumulative Layout Shift (ayrıca hesaplanacak)
        
        // Kaynak metrikleri
        totalResources: performance.getEntriesByType('resource').length,
        totalTransferSize: performance.getEntriesByType('resource')
          .reduce((total, resource) => total + (resource.transferSize || 0), 0)
      };
    });
    
    // LCP hesapla
    const lcp = await this.measureLCP();
    metrics.lcp = lcp;
    
    // CLS hesapla
    const cls = await this.measureCLS();
    metrics.cls = cls;
    
    this.metrics.push({
      url,
      timestamp: Date.now(),
      ...metrics
    });
    
    return metrics;
  }
  
  async measureLCP() {
    return await this.page.evaluate(() => {
      return new Promise((resolve) => {
        const observer = new PerformanceObserver((list) => {
          const entries = list.getEntries();
          const lastEntry = entries[entries.length - 1];
          resolve(lastEntry.startTime);
        });
        observer.observe({ entryTypes: ['largest-contentful-paint'] });
        
        // Timeout
        setTimeout(() => resolve(0), 10000);
      });
    });
  }
  
  async measureCLS() {
    return await this.page.evaluate(() => {
      return new Promise((resolve) => {
        let cls = 0;
        
        const observer = new PerformanceObserver((list) => {
          for (const entry of list.getEntries()) {
            if (!entry.hadRecentInput) {
              cls += entry.value;
            }
          }
        });
        
        observer.observe({ entryTypes: ['layout-shift'] });
        
        setTimeout(() => {
          resolve(cls);
        }, 5000);
      });
    });
  }
  
  generatePerformanceReport() {
    if (this.metrics.length === 0) return null;
    
    const avgMetrics = this.metrics.reduce((acc, metric) => {
      Object.keys(metric).forEach(key => {
        if (typeof metric[key] === 'number' && key !== 'timestamp') {
          acc[key] = (acc[key] || 0) + metric[key];
        }
      });
      return acc;
    }, {});
    
    Object.keys(avgMetrics).forEach(key => {
      avgMetrics[key] = avgMetrics[key] / this.metrics.length;
    });
    
    return {
      totalTests: this.metrics.length,
      averageMetrics: avgMetrics,
      recommendations: this.generateRecommendations(avgMetrics),
      detailedMetrics: this.metrics
    };
  }
  
  generateRecommendations(metrics) {
    const recommendations = [];
    
    if (metrics.ttfb > 600) {
      recommendations.push('TTFB (Time to First Byte) yüksek. Sunucu optimizasyonu gerekli.');
    }
    
    if (metrics.fcp > 2500) {
      recommendations.push('First Contentful Paint yavaş. Critical CSS optimizasyonu yapın.');
    }
    
    if (metrics.lcp > 4000) {
      recommendations.push('Largest Contentful Paint çok yavaş. Görsel optimizasyonu gerekli.');
    }
    
    if (metrics.cls > 0.25) {
      recommendations.push('Cumulative Layout Shift yüksek. Layout kararlılığını artırın.');
    }
    
    if (metrics.totalTransferSize > 2000000) {
      recommendations.push('Toplam transfer boyutu yüksek. Resource optimizasyonu yapın.');
    }
    
    return recommendations;
  }
}
```

---

## 12. En İyi Uygulamalar

### Güvenlik ve Etik Kurallar

```javascript
class EthicalScraper {
  constructor() {
    this.robotsCache = new Map();
    this.rateLimiters = new Map();
  }
  
  async checkRobotsTxt(domain) {
    if (this.robotsCache.has(domain)) {
      return this.robotsCache.get(domain);
    }
    
    try {
      const robotsUrl = `https://${domain}/robots.txt`;
      const response = await fetch(robotsUrl);
      
      if (response.ok) {
        const robotsText = await response.text();
        const rules = this.parseRobotsTxt(robotsText);
        this.robotsCache.set(domain, rules);
        return rules;
      }
    } catch (error) {
      console.log(`robots.txt okunamadı: ${domain}`);
    }
    
    return { allowed: true, crawlDelay: 1 };
  }
  
  parseRobotsTxt(robotsText) {
    const rules = { allowed: true, crawlDelay: 1, disallowed: [] };
    const lines = robotsText.split('\n');
    
    let currentUserAgent = null;
    
    for (const line of lines) {
      const trimmed = line.trim().toLowerCase();
      
      if (trimmed.startsWith('user-agent:')) {
        currentUserAgent = trimmed.split(':')[1].trim();
      } else if (trimmed.startsWith('disallow:') && 
                (currentUserAgent === '*' || currentUserAgent === 'puppeteer')) {
        const path = trimmed.split(':')[1].trim();
        rules.disallowed.push(path);
      } else if (trimmed.startsWith('crawl-delay:')) {
        const delay = parseInt(trimmed.split(':')[1].trim()) * 1000;
        rules.crawlDelay = delay;
      }
    }
    
    return rules;
  }
  
  async isPathAllowed(url) {
    const urlObj = new URL(url);
    const domain = urlObj.hostname;
    const path = urlObj.pathname;
    
    const rules = await this.checkRobotsTxt(domain);
    
    for (const disallowedPath of rules.disallowed) {
      if (path.startsWith(disallowedPath)) {
        return false;
      }
    }
    
    return true;
  }
  
  async respectRateLimit(domain) {
    if (!this.rateLimiters.has(domain)) {
      this.rateLimiters.set(domain, new RateLimiter(10, 60000));
    }
    
    const rateLimiter = this.rateLimiters.get(domain);
    await rateLimiter.waitIfNeeded();
    
    // robots.txt crawl-delay kontrolü
    const rules = await this.checkRobotsTxt(domain);
    if (rules.crawlDelay > 1000) {
      await new Promise(resolve => setTimeout(resolve, rules.crawlDelay));
    }
  }
}
```

### Hata Yönetimi ve Retry Logic

```javascript
class ErrorHandler {
  static async retryOperation(operation, maxRetries = 3, baseDelay = 1000) {
    let lastError;
    
    for (let attempt = 1; attempt <= maxRetries; attempt++) {
      try {
        return await operation();
      } catch (error) {
        lastError = error;
        
        console.log(`Deneme ${attempt}/${maxRetries} başarısız: ${error.message}`);
        
        // Son deneme değilse bekle
        if (attempt < maxRetries) {
          const delay = baseDelay * Math.pow(2, attempt - 1); // Exponential backoff
          await new Promise(resolve => setTimeout(resolve, delay));
        }
      }
    }
    
    throw lastError;
  }
  
  static async handlePageErrors(page) {
    page.on('error', error => {
      console.error('Sayfa hatası:', error.message);
    });
    
    page.on('pageerror', error => {
      console.error('JavaScript hatası:', error.message);
    });
    
    page.on('requestfailed', request => {
      console.error('İstek başarısız:', request.url(), request.failure().errorText);
    });
    
    page.on('response', response => {
      if (response.status() >= 400) {
        console.warn(`HTTP hata: ${response.status()} ${response.url()}`);
      }
    });
  }
  
  static createRobustScraper(scrapingFunction) {
    return async (page, url, options = {}) => {
      const maxRetries = options.maxRetries || 3;
      const timeout = options.timeout || 30000;
      
      return await this.retryOperation(async () => {
        // Timeout kontrolü
        const timeoutPromise = new Promise((_, reject) => {
          setTimeout(() => reject(new Error('Scraping timeout')), timeout);
        });
        
        // Scraping işlemi
        const scrapingPromise = scrapingFunction(page, url, options);
        
        return await Promise.race([scrapingPromise, timeoutPromise]);
      }, maxRetries);
    };
  }
}
```

### Kapsamlı Örnek Proje

```javascript
class ComprehensiveEcommerceScraper {
  constructor(options = {}) {
    this.options = {
      headless: 'new',
      maxConcurrency: 5,
      requestDelay: 2000,
      maxRetries: 3,
      outputFormat: 'json',
      ...options
    };
    
    this.browser = null;
    this.ethicalScraper = new EthicalScraper();
    this.monitor = new ScrapingMonitor();
    this.parallelScraper = new ParallelScraper(this.options.maxConcurrency);
  }
  
  async initialize() {
    this.browser = await StealthScraper.launch();
    await this.parallelScraper.initialize();
    
    console.log('Scraper başlatıldı');
  }
  
  async scrapeProductListings(urls) {
    const results = [];
    
    for (const url of urls) {
      try {
        const urlObj = new URL(url);
        const domain = urlObj.hostname;
        
        // Etik kontroller
        const isAllowed = await this.ethicalScraper.isPathAllowed(url);
        if (!isAllowed) {
          console.log(`URL robots.txt tarafından yasaklanmış: ${url}`);
          continue;
        }
        
        await this.ethicalScraper.respectRateLimit(domain);
        
        // Scraping işlemi
        this.monitor.logRequest(url);
        const startTime = Date.now();
        
        const productData = await ErrorHandler.retryOperation(async () => {
          return await this.scrapeProducts(url);
        }, this.options.maxRetries);
        
        const duration = Date.now() - startTime;
        this.monitor.logSuccess(url, duration, JSON.stringify(productData).length);
        
        results.push({
          url,
          success: true,
          data: productData,
          timestamp: new Date().toISOString()
        });
        
      } catch (error) {
        const duration = Date.now() - startTime;
        this.monitor.logError(url, error, duration);
        
        results.push({
          url,
          success: false,
          error: error.message,
          timestamp: new Date().toISOString()
        });
      }
    }
    
    return results;
  }
  
  async scrapeProducts(url) {
    const page = await this.browser.newPage();
    
    try {
      await StealthScraper.setupStealthPage(page);
      await ErrorHandler.handlePageErrors(page);
      
      // Sayfa yükleme
      await page.goto(url, { waitUntil: 'networkidle2' });
      
      // Cloudflare kontrolü
      const cfBypass = new CloudflareBypass(page);
      if (await cfBypass.detectCloudflare()) {
        await cfBypass.waitForCloudflareBypass();
      }
      
      // Ürün verilerini çek
      const products = await page.evaluate(() => {
        const productElements = document.querySelectorAll('.product-item, .product-card, [data-testid="product"]');
        
        return Array.from(productElements).map(product => {
          const getTextContent = (selector) => {
            const element = product.querySelector(selector);
            return element ? element.textContent.trim() : null;
          };
          
          const getAttribute = (selector, attr) => {
            const element = product.querySelector(selector);
            return element ? element.getAttribute(attr) : null;
          };
          
          return {
            title: getTextContent('h3, .title, .product-title, .product-name'),
            price: getTextContent('.price, .product-price, [data-testid="price"]'),
            originalPrice: getTextContent('.original-price, .old-price, .was-price'),
            discount: getTextContent('.discount, .sale-badge, .discount-percentage'),
            rating: getTextContent('.rating, .stars, [data-testid="rating"]'),
            reviewCount: getTextContent('.review-count, .reviews, [data-testid="review-count"]'),
            availability: getTextContent('.availability, .stock-status, [data-testid="stock"]'),
            image: getAttribute('img', 'src') || getAttribute('img', 'data-src'),
            link: getAttribute('a', 'href'),
            brand: getTextContent('.brand, .manufacturer, [data-testid="brand"]'),
            category: getTextContent('.category, .breadcrumb, [data-testid="category"]'),
            sku: getAttribute('[data-sku]', 'data-sku') || getTextContent('.sku, .product-id'),
            features: Array.from(product.querySelectorAll('.feature, .spec, .highlight')).map(el => el.textContent.trim()),
            shipping: getTextContent('.shipping, .delivery, [data-testid="shipping"]'),
            seller: getTextContent('.seller, .vendor, [data-testid="seller"]'),
            condition: getTextContent('.condition, .item-condition, [data-testid="condition"]')
          };
        }).filter(product => product.title); // Boş ürünleri filtrele
      });
      
      // Pagination kontrolü
      const hasNextPage = await page.$('.pagination .next, .next-page, [data-testid="next-page"]') !== null;
      
      return {
        products,
        hasNextPage,
        totalProducts: products.length,
        scrapedAt: new Date().toISOString()
      };
      
    } finally {
      await page.close();
    }
  }
  
  async exportResults(results, filename) {
    const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
    const fullFilename = `${filename}-${timestamp}`;
    
    // Başarılı sonuçları ayır
    const successfulResults = results.filter(r => r.success);
    const failedResults = results.filter(r => !r.success);
    
    // Tüm ürünleri tek array'de topla
    const allProducts = successfulResults.flatMap(r => r.data.products);
    
    // JSON export
    await DataExporter.saveAsJSON({
      summary: {
        totalUrls: results.length,
        successfulUrls: successfulResults.length,
        failedUrls: failedResults.length,
        totalProducts: allProducts.length,
        scrapedAt: new Date().toISOString()
      },
      products: allProducts,
      failed: failedResults,
      statistics: this.monitor.getStats()
    }, fullFilename);
    
    // CSV export (sadece ürünler)
    if (allProducts.length > 0) {
      const csvHeaders = Object.keys(allProducts[0]).map(key => ({
        id: key,
        title: key.charAt(0).toUpperCase() + key.slice(1)
      }));
      
      await DataExporter.saveAsCSV(allProducts, fullFilename, csvHeaders);
    }
    
    console.log(`Sonuçlar kaydedildi: ${fullFilename}`);
  }
  
  async close() {
    if (this.browser) {
      await this.browser.close();
    }
    
    await this.parallelScraper.close();
  }
}

// Kullanım örneği
async function main() {
  const scraper = new ComprehensiveEcommerceScraper({
    headless: false,
    maxConcurrency: 3,
    requestDelay: 3000
  });
  
  try {
    await scraper.initialize();
    
    const urls = [
      'https://example-shop.com/products',
      'https://example-shop.com/electronics',
      'https://example-shop.com/clothing'
    ];
    
    console.log('Scraping başlatıldı...');
    const results = await scraper.scrapeProductListings(urls);
    
    console.log('Sonuçlar export ediliyor...');
    await scraper.exportResults(results, 'ecommerce-products');
    
    console.log('İstatistikler:', scraper.monitor.getStats());
    
  } catch (error) {
    console.error('Scraping hatası:', error);
  } finally {
    await scraper.close();
  }
}

// Çalıştır
if (require.main === module) {
  main().catch(console.error);
}
```

---

## 13. Veri Saklama ve Veritabanı Entegrasyonu

### MongoDB Entegrasyonu

```javascript
import { MongoClient } from 'mongodb';
import mongoose from 'mongoose';

// MongoDB Connection Manager
class MongoDBManager {
  constructor(connectionString = 'mongodb://localhost:27017/scraping') {
    this.connectionString = connectionString;
    this.client = null;
    this.db = null;
  }
  
  async connect() {
    try {
      this.client = new MongoClient(this.connectionString);
      await this.client.connect();
      this.db = this.client.db();
      console.log('MongoDB bağlantısı başarılı');
    } catch (error) {
      console.error('MongoDB bağlantı hatası:', error);
      throw error;
    }
  }
  
  async disconnect() {
    if (this.client) {
      await this.client.close();
      console.log('MongoDB bağlantısı kapatıldı');
    }
  }
  
  async saveScrapedData(collectionName, data, options = {}) {
    const collection = this.db.collection(collectionName);
    
    // Duplicate kontrolü
    if (options.upsert && options.uniqueField) {
      const filter = { [options.uniqueField]: data[options.uniqueField] };
      return await collection.replaceOne(filter, data, { upsert: true });
    }
    
    // Bulk insert için
    if (Array.isArray(data)) {
      return await collection.insertMany(data, { ordered: false });
    }
    
    // Single insert
    return await collection.insertOne({
      ...data,
      createdAt: new Date(),
      scrapedAt: new Date()
    });
  }
  
  async findData(collectionName, query = {}, options = {}) {
    const collection = this.db.collection(collectionName);
    
    let cursor = collection.find(query);
    
    if (options.sort) cursor = cursor.sort(options.sort);
    if (options.limit) cursor = cursor.limit(options.limit);
    if (options.skip) cursor = cursor.skip(options.skip);
    
    return await cursor.toArray();
  }
  
  async aggregateData(collectionName, pipeline) {
    const collection = this.db.collection(collectionName);
    return await collection.aggregate(pipeline).toArray();
  }
}

// Mongoose Schema Örnekleri
const productSchema = new mongoose.Schema({
  title: { type: String, required: true, index: true },
  price: { type: Number, index: true },
  originalPrice: Number,
  discount: String,
  rating: Number,
  reviewCount: Number,
  availability: String,
  image: String,
  link: { type: String, unique: true },
  brand: { type: String, index: true },
  category: { type: String, index: true },
  sku: { type: String, unique: true },
  features: [String],
  shipping: String,
  seller: String,
  condition: String,
  source: { type: String, required: true },
  scrapedAt: { type: Date, default: Date.now, index: true },
  lastUpdated: { type: Date, default: Date.now }
});

// TTL index - 30 gün sonra otomatik sil
productSchema.index({ scrapedAt: 1 }, { expireAfterSeconds: 30 * 24 * 60 * 60 });

const Product = mongoose.model('Product', productSchema);

class ProductRepository {
  async saveProduct(productData) {
    try {
      const product = new Product({
        ...productData,
        lastUpdated: new Date()
      });
      
      return await product.save();
    } catch (error) {
      if (error.code === 11000) {
        // Duplicate key error - update existing
        return await Product.findOneAndUpdate(
          { $or: [{ link: productData.link }, { sku: productData.sku }] },
          { ...productData, lastUpdated: new Date() },
          { new: true, upsert: true }
        );
      }
      throw error;
    }
  }
  
  async findProductsByPriceRange(minPrice, maxPrice) {
    return await Product.find({
      price: { $gte: minPrice, $lte: maxPrice }
    }).sort({ price: 1 });
  }
  
  async getProductTrends(category, days = 30) {
    const since = new Date(Date.now() - days * 24 * 60 * 60 * 1000);
    
    return await Product.aggregate([
      {
        $match: {
          category: category,
          scrapedAt: { $gte: since }
        }
      },
      {
        $group: {
          _id: {
            $dateToString: { format: "%Y-%m-%d", date: "$scrapedAt" }
          },
          avgPrice: { $avg: "$price" },
          count: { $sum: 1 }
        }
      },
      { $sort: { "_id": 1 } }
    ]);
  }
}
```

### PostgreSQL Entegrasyonu

```javascript
import pg from 'pg';
import { Sequelize, DataTypes } from 'sequelize';

class PostgreSQLManager {
  constructor(config = {}) {
    this.config = {
      host: process.env.DB_HOST || 'localhost',
      port: process.env.DB_PORT || 5432,
      database: process.env.DB_NAME || 'scraping',
      username: process.env.DB_USER || 'postgres',
      password: process.env.DB_PASS || 'password',
      ...config
    };
    
    this.sequelize = new Sequelize(
      this.config.database,
      this.config.username,
      this.config.password,
      {
        host: this.config.host,
        port: this.config.port,
        dialect: 'postgres',
        logging: false,
        pool: {
          max: 10,
          min: 0,
          acquire: 30000,
          idle: 10000
        }
      }
    );
  }
  
  async connect() {
    try {
      await this.sequelize.authenticate();
      console.log('PostgreSQL bağlantısı başarılı');
    } catch (error) {
      console.error('PostgreSQL bağlantı hatası:', error);
      throw error;
    }
  }
  
  async disconnect() {
    await this.sequelize.close();
    console.log('PostgreSQL bağlantısı kapatıldı');
  }
  
  // Model tanımları
  defineModels() {
    const Product = this.sequelize.define('Product', {
      id: {
        type: DataTypes.UUID,
        defaultValue: DataTypes.UUIDV4,
        primaryKey: true
      },
      title: {
        type: DataTypes.STRING,
        allowNull: false,
        validate: { notEmpty: true }
      },
      price: {
        type: DataTypes.DECIMAL(10, 2),
        validate: { min: 0 }
      },
      originalPrice: DataTypes.DECIMAL(10, 2),
      discount: DataTypes.STRING,
      rating: {
        type: DataTypes.FLOAT,
        validate: { min: 0, max: 5 }
      },
      reviewCount: DataTypes.INTEGER,
      availability: DataTypes.STRING,
      image: DataTypes.TEXT,
      link: {
        type: DataTypes.TEXT,
        unique: true,
        validate: { isUrl: true }
      },
      brand: DataTypes.STRING,
      category: DataTypes.STRING,
      sku: {
        type: DataTypes.STRING,
        unique: true
      },
      features: DataTypes.JSONB,
      shipping: DataTypes.STRING,
      seller: DataTypes.STRING,
      condition: DataTypes.STRING,
      source: {
        type: DataTypes.STRING,
        allowNull: false
      },
      metadata: DataTypes.JSONB,
      scrapedAt: {
        type: DataTypes.DATE,
        defaultValue: DataTypes.NOW
      }
    }, {
      indexes: [
        { fields: ['brand'] },
        { fields: ['category'] },
        { fields: ['price'] },
        { fields: ['scrapedAt'] },
        { fields: ['source'] }
      ]
    });
    
    const ScrapingJob = this.sequelize.define('ScrapingJob', {
      id: {
        type: DataTypes.UUID,
        defaultValue: DataTypes.UUIDV4,
        primaryKey: true
      },
      url: {
        type: DataTypes.TEXT,
        allowNull: false
      },
      status: {
        type: DataTypes.ENUM('pending', 'running', 'completed', 'failed'),
        defaultValue: 'pending'
      },
      totalItems: DataTypes.INTEGER,
      processedItems: DataTypes.INTEGER,
      errors: DataTypes.JSONB,
      startedAt: DataTypes.DATE,
      completedAt: DataTypes.DATE
    });
    
    return { Product, ScrapingJob };
  }
  
  async sync() {
    await this.sequelize.sync({ alter: true });
    console.log('Veritabanı tabloları senkronize edildi');
  }
}
```

### Redis Önbellek Sistemi

```javascript
import Redis from 'ioredis';

class RedisCache {
  constructor(config = {}) {
    this.redis = new Redis({
      host: process.env.REDIS_HOST || 'localhost',
      port: process.env.REDIS_PORT || 6379,
      password: process.env.REDIS_PASSWORD,
      db: process.env.REDIS_DB || 0,
      retryDelayOnFailover: 100,
      maxRetriesPerRequest: 3,
      ...config
    });
    
    this.defaultExpiry = 3600; // 1 saat
  }
  
  async set(key, value, expiry = this.defaultExpiry) {
    const serialized = JSON.stringify(value);
    if (expiry > 0) {
      return await this.redis.setex(key, expiry, serialized);
    }
    return await this.redis.set(key, serialized);
  }
  
  async get(key) {
    const value = await this.redis.get(key);
    return value ? JSON.parse(value) : null;
  }
  
  async has(key) {
    return (await this.redis.exists(key)) === 1;
  }
  
  async delete(key) {
    return await this.redis.del(key);
  }
  
  async increment(key, amount = 1) {
    return await this.redis.incrby(key, amount);
  }
  
  // Cache-aside pattern
  async getOrSet(key, fetchFunction, expiry = this.defaultExpiry) {
    let value = await this.get(key);
    
    if (value === null) {
      value = await fetchFunction();
      if (value !== null) {
        await this.set(key, value, expiry);
      }
    }
    
    return value;
  }
  
  // Rate limiting için
  async isRateLimited(identifier, limit, windowSeconds) {
    const key = `rate_limit:${identifier}`;
    const current = await this.increment(key);
    
    if (current === 1) {
      await this.redis.expire(key, windowSeconds);
    }
    
    return current > limit;
  }
}

// Kullanım örneği
class CachedProductScraper {
  constructor(dbManager, cache) {
    this.db = dbManager;
    this.cache = cache;
  }
  
  async scrapeProductWithCache(url) {
    const cacheKey = `product:${Buffer.from(url).toString('base64')}`;
    
    // Önce cache'den kontrol et
    let product = await this.cache.get(cacheKey);
    
    if (!product) {
      // Cache miss - scrape ve kaydet
      product = await this.scrapeProduct(url);
      
      // Database'e kaydet
      await this.db.saveProduct(product);
      
      // Cache'e kaydet (1 saat)
      await this.cache.set(cacheKey, product, 3600);
    }
    
    return product;
  }
  
  async getProductStats(category) {
    const cacheKey = `stats:${category}`;
    
    return await this.cache.getOrSet(cacheKey, async () => {
      return await this.db.aggregateData('products', [
        { $match: { category } },
        {
          $group: {
            _id: null,
            avgPrice: { $avg: '$price' },
            maxPrice: { $max: '$price' },
            minPrice: { $min: '$price' },
            count: { $sum: 1 }
          }
        }
      ]);
    }, 1800); // 30 dakika cache
  }
}
```

---

## 14. Environment ve Konfigürasyon Yönetimi

### Environment Variables

```javascript
import dotenv from 'dotenv';
import Joi from 'joi';
import fs from 'fs';

// .env dosyası yükleme
dotenv.config();

// Environment validation schema
const envSchema = Joi.object({
  NODE_ENV: Joi.string().valid('development', 'production', 'test').default('development'),
  
  // Database
  DB_HOST: Joi.string().default('localhost'),
  DB_PORT: Joi.number().default(5432),
  DB_NAME: Joi.string().required(),
  DB_USER: Joi.string().required(),
  DB_PASS: Joi.string().required(),
  
  // Redis
  REDIS_HOST: Joi.string().default('localhost'),
  REDIS_PORT: Joi.number().default(6379),
  REDIS_PASSWORD: Joi.string().allow(''),
  
  // Puppeteer
  CHROME_EXECUTABLE_PATH: Joi.string().allow(''),
  MAX_CONCURRENCY: Joi.number().default(5),
  HEADLESS_MODE: Joi.boolean().default(true),
  
  // Scraping
  DEFAULT_TIMEOUT: Joi.number().default(30000),
  MAX_RETRIES: Joi.number().default(3),
  RATE_LIMIT_REQUESTS: Joi.number().default(10),
  RATE_LIMIT_WINDOW: Joi.number().default(60000),
  
  // Notifications
  SLACK_WEBHOOK_URL: Joi.string().uri().allow(''),
  DISCORD_WEBHOOK_URL: Joi.string().uri().allow(''),
  SMTP_HOST: Joi.string().allow(''),
  SMTP_PORT: Joi.number().allow(''),
  SMTP_USER: Joi.string().allow(''),
  SMTP_PASS: Joi.string().allow(''),
  
  // Security
  API_KEY: Joi.string().required(),
  JWT_SECRET: Joi.string().required(),
  
  // Monitoring
  LOG_LEVEL: Joi.string().valid('error', 'warn', 'info', 'debug').default('info'),
  METRICS_PORT: Joi.number().default(9090)
}).unknown();

class Config {
  constructor() {
    this.validate();
    this.load();
  }
  
  validate() {
    const { error, value } = envSchema.validate(process.env);
    
    if (error) {
      throw new Error(`Environment validation error: ${error.message}`);
    }
    
    this.env = value;
  }
  
  load() {
    this.database = {
      host: this.env.DB_HOST,
      port: this.env.DB_PORT,
      database: this.env.DB_NAME,
      username: this.env.DB_USER,
      password: this.env.DB_PASS
    };
    
    this.redis = {
      host: this.env.REDIS_HOST,
      port: this.env.REDIS_PORT,
      password: this.env.REDIS_PASSWORD || undefined
    };
    
    this.puppeteer = {
      executablePath: this.env.CHROME_EXECUTABLE_PATH || undefined,
      headless: this.env.HEADLESS_MODE,
      args: this.getChromiumArgs()
    };
    
    this.scraping = {
      maxConcurrency: this.env.MAX_CONCURRENCY,
      defaultTimeout: this.env.DEFAULT_TIMEOUT,
      maxRetries: this.env.MAX_RETRIES,
      rateLimit: {
        requests: this.env.RATE_LIMIT_REQUESTS,
        window: this.env.RATE_LIMIT_WINDOW
      }
    };
    
    this.notifications = {
      slack: { webhookUrl: this.env.SLACK_WEBHOOK_URL },
      discord: { webhookUrl: this.env.DISCORD_WEBHOOK_URL },
      email: {
        host: this.env.SMTP_HOST,
        port: this.env.SMTP_PORT,
        auth: {
          user: this.env.SMTP_USER,
          pass: this.env.SMTP_PASS
        }
      }
    };
    
    this.security = {
      apiKey: this.env.API_KEY,
      jwtSecret: this.env.JWT_SECRET
    };
    
    this.monitoring = {
      logLevel: this.env.LOG_LEVEL,
      metricsPort: this.env.METRICS_PORT
    };
  }
  
  getChromiumArgs() {
    const args = [
      '--no-sandbox',
      '--disable-setuid-sandbox',
      '--disable-dev-shm-usage',
      '--disable-web-security',
      '--disable-extensions'
    ];
    
    if (this.env.NODE_ENV === 'production') {
      args.push('--disable-gpu', '--disable-dev-shm-usage');
    }
    
    return args;
  }
  
  isDevelopment() {
    return this.env.NODE_ENV === 'development';
  }
  
  isProduction() {
    return this.env.NODE_ENV === 'production';
  }
  
  isTest() {
    return this.env.NODE_ENV === 'test';
  }
}

// Singleton pattern
let configInstance = null;

export function getConfig() {
  if (!configInstance) {
    configInstance = new Config();
  }
  return configInstance;
}
```

### Yapılandırma Dosyaları

```javascript
// config/scraping-targets.js
export const scrapingTargets = {
  ecommerce: [
    {
      name: 'Example Shop',
      baseUrl: 'https://example-shop.com',
      productListUrls: [
        '/electronics',
        '/clothing',
        '/home-garden'
      ],
      selectors: {
        productItem: '.product-item',
        title: '.title',
        price: '.price',
        image: 'img',
        link: 'a'
      },
      rateLimit: {
        requests: 5,
        window: 60000
      },
      headers: {
        'User-Agent': 'Mozilla/5.0 (compatible; ShopBot/1.0)',
        'Accept': 'text/html,application/xhtml+xml'
      }
    }
  ],
  
  social: [
    {
      name: 'Social Platform',
      baseUrl: 'https://social.example.com',
      endpoints: {
        posts: '/api/posts',
        users: '/api/users'
      },
      authentication: {
        type: 'oauth',
        credentials: {
          clientId: process.env.SOCIAL_CLIENT_ID,
          clientSecret: process.env.SOCIAL_CLIENT_SECRET
        }
      }
    }
  ]
};

// config/user-agents.js
export const userAgents = [
  'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36',
  'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36',
  'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36',
  'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/121.0',
  'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/121.0'
];

export const mobileUserAgents = [
  'Mozilla/5.0 (iPhone; CPU iPhone OS 17_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Mobile/15E148 Safari/604.1',
  'Mozilla/5.0 (Linux; Android 13; SM-S918B) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Mobile Safari/537.36',
  'Mozilla/5.0 (iPad; CPU OS 17_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Mobile/15E148 Safari/604.1'
];

// config/proxy-list.js
export const proxies = [
  {
    host: 'proxy1.example.com',
    port: 8080,
    username: process.env.PROXY_USER,
    password: process.env.PROXY_PASS,
    country: 'US'
  },
  {
    host: 'proxy2.example.com',
    port: 8080,
    username: process.env.PROXY_USER,
    password: process.env.PROXY_PASS,
    country: 'UK'
  }
];
```

### Dinamik Konfigürasyon

```javascript
import fs from 'fs/promises';
import chokidar from 'chokidar';

class DynamicConfig {
  constructor(configPath = './config/runtime-config.json') {
    this.configPath = configPath;
    this.config = {};
    this.watchers = new Set();
    this.changeCallbacks = new Set();
  }
  
  async load() {
    try {
      const configData = await fs.readFile(this.configPath, 'utf8');
      this.config = JSON.parse(configData);
      console.log('Konfigürasyon yüklendi');
    } catch (error) {
      console.warn('Konfigürasyon dosyası bulunamadı, varsayılan değerler kullanılıyor');
      this.config = this.getDefaultConfig();
      await this.save();
    }
    
    this.watchForChanges();
  }
  
  async save() {
    const configData = JSON.stringify(this.config, null, 2);
    await fs.writeFile(this.configPath, configData);
  }
  
  get(path, defaultValue = null) {
    const keys = path.split('.');
    let value = this.config;
    
    for (const key of keys) {
      if (value && typeof value === 'object' && key in value) {
        value = value[key];
      } else {
        return defaultValue;
      }
    }
    
    return value;
  }
  
  set(path, value) {
    const keys = path.split('.');
    const lastKey = keys.pop();
    let current = this.config;
    
    for (const key of keys) {
      if (!(key in current)) {
        current[key] = {};
      }
      current = current[key];
    }
    
    current[lastKey] = value;
    this.save();
  }
  
  watchForChanges() {
    const watcher = chokidar.watch(this.configPath);
    
    watcher.on('change', async () => {
      console.log('Konfigürasyon değişikliği algılandı, yeniden yükleniyor...');
      await this.load();
      this.notifyChanges();
    });
    
    this.watchers.add(watcher);
  }
  
  onChange(callback) {
    this.changeCallbacks.add(callback);
  }
  
  notifyChanges() {
    for (const callback of this.changeCallbacks) {
      try {
        callback(this.config);
      } catch (error) {
        console.error('Config change callback error:', error);
      }
    }
  }
  
  getDefaultConfig() {
    return {
      scraping: {
        maxConcurrency: 3,
        delayBetweenRequests: 2000,
        timeout: 30000,
        retryAttempts: 3,
        rotateUserAgents: true,
        respectRobotsTxt: true
      },
      notifications: {
        enabled: true,
        channels: ['console'],
        thresholds: {
          errorRate: 0.1,
          successRate: 0.8
        }
      },
      storage: {
        format: 'json',
        compression: false,
        backup: true,
        retention: {
          days: 30
        }
      }
    };
  }
}

// Singleton export
export const dynamicConfig = new DynamicConfig();
```

---

## 15. Veri Doğrulama ve Temizleme

### Veri Validation

```javascript
import Joi from 'joi';
import validator from 'validator';

class DataValidator {
  constructor() {
    this.schemas = this.defineSchemas();
  }
  
  defineSchemas() {
    return {
      product: Joi.object({
        title: Joi.string().trim().min(1).max(500).required(),
        price: Joi.number().positive().precision(2).allow(null),
        originalPrice: Joi.number().positive().precision(2).allow(null),
        discount: Joi.string().trim().max(50).allow(null),
        rating: Joi.number().min(0).max(5).allow(null),
        reviewCount: Joi.number().integer().min(0).allow(null),
        availability: Joi.string().trim().max(100).allow(null),
        image: Joi.string().uri().allow(null, ''),
        link: Joi.string().uri().required(),
        brand: Joi.string().trim().max(100).allow(null),
        category: Joi.string().trim().max(100).allow(null),
        sku: Joi.string().trim().max(50).allow(null),
        features: Joi.array().items(Joi.string().trim()).allow(null),
        shipping: Joi.string().trim().max(200).allow(null),
        seller: Joi.string().trim().max(100).allow(null),
        condition: Joi.string().valid('new', 'used', 'refurbished').allow(null),
        source: Joi.string().trim().required()
      }),
      
      user: Joi.object({
        username: Joi.string().alphanum().min(3).max(30).required(),
        email: Joi.string().email().required(),
        fullName: Joi.string().trim().min(2).max(100),
        bio: Joi.string().max(1000).allow(''),
        followers: Joi.number().integer().min(0),
        following: Joi.number().integer().min(0),
        verified: Joi.boolean(),
        avatar: Joi.string().uri().allow(null, ''),
        url: Joi.string().uri().allow(null, ''),
        location: Joi.string().trim().max(100).allow(null),
        joinDate: Joi.date().allow(null)
      }),
      
      post: Joi.object({
        id: Joi.string().required(),
        content: Joi.string().max(5000).required(),
        author: Joi.string().required(),
        timestamp: Joi.date().required(),
        likes: Joi.number().integer().min(0),
        shares: Joi.number().integer().min(0),
        comments: Joi.number().integer().min(0),
        media: Joi.array().items(Joi.string().uri()),
        hashtags: Joi.array().items(Joi.string().pattern(/^#\w+$/)),
        mentions: Joi.array().items(Joi.string().pattern(/^@\w+$/)),
        url: Joi.string().uri().required()
      })
    };
  }
  
  validate(data, schemaName) {
    const schema = this.schemas[schemaName];
    if (!schema) {
      throw new Error(`Schema '${schemaName}' bulunamadı`);
    }
    
    const { error, value } = schema.validate(data, {
      stripUnknown: true,
      abortEarly: false
    });
    
    return {
      isValid: !error,
      data: value,
      errors: error ? error.details.map(detail => ({
        field: detail.path.join('.'),
        message: detail.message,
        value: detail.context.value
      })) : []
    };
  }
  
  validateBatch(dataArray, schemaName) {
    const results = {
      valid: [],
      invalid: [],
      summary: {
        total: dataArray.length,
        validCount: 0,
        invalidCount: 0
      }
    };
    
    for (const item of dataArray) {
      const validation = this.validate(item, schemaName);
      
      if (validation.isValid) {
        results.valid.push(validation.data);
        results.summary.validCount++;
      } else {
        results.invalid.push({
          originalData: item,
          errors: validation.errors
        });
        results.summary.invalidCount++;
      }
    }
    
    return results;
  }
}
```

### Veri Temizleme

```javascript
import DOMPurify from 'isomorphic-dompurify';
import { decode } from 'html-entities';

class DataCleaner {
  constructor() {
    this.pricePatterns = [
      /[\$€£¥₺]/g,
      /[^\d.,]/g
    ];
    
    this.currencySymbols = {
      '$': 'USD',
      '€': 'EUR',
      '£': 'GBP',
      '¥': 'JPY',
      '₺': 'TRY'
    };
  }
  
  cleanText(text) {
    if (!text || typeof text !== 'string') return '';
    
    return text
      .trim()
      .replace(/\s+/g, ' ') // Çoklu boşlukları tek boşluğa çevir
      .replace(/[\r\n\t]/g, ' ') // Satır sonları ve tab'ları boşluğa çevir
      .replace(/[^\x20-\x7E\u00A0-\uFFFF]/g, '') // Kontrol karakterlerini kaldır
      .trim();
  }
  
  cleanHTML(html) {
    if (!html || typeof html !== 'string') return '';
    
    // HTML entities'i decode et
    let cleaned = decode(html);
    
    // HTML'i temizle
    cleaned = DOMPurify.sanitize(cleaned, {
      ALLOWED_TAGS: [],
      ALLOWED_ATTR: []
    });
    
    return this.cleanText(cleaned);
  }
  
  cleanPrice(priceText) {
    if (!priceText || typeof priceText !== 'string') return null;
    
    // Currency detection
    let currency = 'USD';
    for (const [symbol, code] of Object.entries(this.currencySymbols)) {
      if (priceText.includes(symbol)) {
        currency = code;
        break;
      }
    }
    
    // Sayısal değeri çıkar
    let numericValue = priceText
      .replace(/[^\d.,]/g, '')
      .replace(/,(?=\d{3})/g, '') // Binlik ayıraçlarını kaldır
      .replace(/,/g, '.'); // Virgülü noktaya çevir
    
    const parsed = parseFloat(numericValue);
    
    return isNaN(parsed) ? null : {
      value: parsed,
      currency: currency,
      original: priceText
    };
  }
  
  cleanRating(ratingText) {
    if (!ratingText || typeof ratingText !== 'string') return null;
    
    // Sayısal değeri çıkar
    const match = ratingText.match(/(\d+(?:\.\d+)?)/);
    if (match) {
      const rating = parseFloat(match[1]);
      return rating >= 0 && rating <= 5 ? rating : null;
    }
    
    // Yıldız sayısı
    const stars = (ratingText.match(/★/g) || []).length;
    return stars > 0 && stars <= 5 ? stars : null;
  }
  
  cleanUrl(url, baseUrl = '') {
    if (!url || typeof url !== 'string') return null;
    
    try {
      // Relative URL'yi absolute'a çevir
      if (url.startsWith('/') && baseUrl) {
        url = new URL(url, baseUrl).href;
      }
      
      // URL validation
      const urlObj = new URL(url);
      
      // Gereksiz parametreleri kaldır
      const paramsToRemove = ['utm_source', 'utm_medium', 'utm_campaign', 'ref', 'referrer'];
      paramsToRemove.forEach(param => {
        urlObj.searchParams.delete(param);
      });
      
      return urlObj.href;
    } catch (error) {
      return null;
    }
  }
  
  cleanProductData(product) {
    const cleaned = {};
    
    // Text fields
    const textFields = ['title', 'brand', 'category', 'seller', 'condition', 'shipping'];
    textFields.forEach(field => {
      if (product[field]) {
        cleaned[field] = this.cleanText(product[field]);
      }
    });
    
    // HTML fields
    const htmlFields = ['description'];
    htmlFields.forEach(field => {
      if (product[field]) {
        cleaned[field] = this.cleanHTML(product[field]);
      }
    });
    
    // Price fields
    const priceFields = ['price', 'originalPrice'];
    priceFields.forEach(field => {
      if (product[field]) {
        cleaned[field] = this.cleanPrice(product[field]);
      }
    });
    
    // Rating
    if (product.rating) {
      cleaned.rating = this.cleanRating(product.rating);
    }
    
    // URLs
    const urlFields = ['link', 'image'];
    urlFields.forEach(field => {
      if (product[field]) {
        cleaned[field] = this.cleanUrl(product[field]);
      }
    });
    
    // Arrays
    if (product.features && Array.isArray(product.features)) {
      cleaned.features = product.features
        .map(feature => this.cleanText(feature))
        .filter(feature => feature.length > 0);
    }
    
    // Numbers
    const numberFields = ['reviewCount'];
    numberFields.forEach(field => {
      if (product[field]) {
        const num = parseInt(product[field].toString().replace(/[^\d]/g, ''));
        cleaned[field] = isNaN(num) ? null : num;
      }
    });
    
    return cleaned;
  }
  
  detectDuplicates(products, compareFields = ['title', 'sku', 'link']) {
    const seen = new Map();
    const duplicates = [];
    const unique = [];
    
    for (const product of products) {
      const signature = compareFields
        .map(field => (product[field] || '').toString().toLowerCase().trim())
        .join('|');
      
      if (seen.has(signature)) {
        duplicates.push({
          original: seen.get(signature),
          duplicate: product,
          matchedFields: compareFields
        });
      } else {
        seen.set(signature, product);
        unique.push(product);
      }
    }
    
    return { unique, duplicates };
  }
  
  normalizeProductTitles(products) {
    return products.map(product => {
      if (!product.title) return product;
      
      let normalized = product.title
        // Marka adını başa al
        .replace(/^(.+?)\s*-\s*(.+)$/, (match, title, brand) => {
          if (product.brand && title.toLowerCase().includes(product.brand.toLowerCase())) {
            return title;
          }
          return `${brand} - ${title}`;
        })
        // Gereksiz kelimeleri kaldır
        .replace(/\b(new|brand new|original|genuine|authentic)\b/gi, '')
        // Fazla boşlukları temizle
        .replace(/\s+/g, ' ')
        .trim();
      
      return {
        ...product,
        title: normalized,
        originalTitle: product.title
      };
    });
  }
}
```

### Veri Kalitesi Kontrolü

```javascript
class DataQualityChecker {
  constructor() {
    this.qualityRules = {
      product: [
        { field: 'title', required: true, minLength: 5 },
        { field: 'price', required: false, type: 'number', min: 0 },
        { field: 'link', required: true, type: 'url' },
        { field: 'image', required: false, type: 'url' },
        { field: 'rating', required: false, type: 'number', min: 0, max: 5 }
      ]
    };
  }
  
  checkQuality(data, type = 'product') {
    const rules = this.qualityRules[type] || [];
    const issues = [];
    let score = 100;
    
    for (const rule of rules) {
      const value = data[rule.field];
      const issue = this.validateField(value, rule);
      
      if (issue) {
        issues.push(issue);
        score -= issue.severity * 10;
      }
    }
    
    return {
      score: Math.max(0, score),
      grade: this.getGrade(score),
      issues: issues,
      isAcceptable: score >= 60
    };
  }
  
  validateField(value, rule) {
    // Required check
    if (rule.required && (value === null || value === undefined || value === '')) {
      return {
        field: rule.field,
        type: 'missing_required',
        message: `Required field '${rule.field}' is missing`,
        severity: 3
      };
    }
    
    if (value === null || value === undefined || value === '') {
      return null; // Non-required empty fields are ok
    }
    
    // Type check
    if (rule.type) {
      if (rule.type === 'number' && typeof value !== 'number') {
        return {
          field: rule.field,
          type: 'invalid_type',
          message: `Field '${rule.field}' should be a number`,
          severity: 2
        };
      }
      
      if (rule.type === 'url' && !validator.isURL(value.toString())) {
        return {
          field: rule.field,
          type: 'invalid_url',
          message: `Field '${rule.field}' should be a valid URL`,
          severity: 2
        };
      }
    }
    
    // String length check
    if (rule.minLength && typeof value === 'string' && value.length < rule.minLength) {
      return {
        field: rule.field,
        type: 'too_short',
        message: `Field '${rule.field}' is too short (minimum ${rule.minLength} characters)`,
        severity: 1
      };
    }
    
    // Number range check
    if (typeof value === 'number') {
      if (rule.min !== undefined && value < rule.min) {
        return {
          field: rule.field,
          type: 'below_minimum',
          message: `Field '${rule.field}' is below minimum value (${rule.min})`,
          severity: 2
        };
      }
      
      if (rule.max !== undefined && value > rule.max) {
        return {
          field: rule.field,
          type: 'above_maximum',
          message: `Field '${rule.field}' is above maximum value (${rule.max})`,
          severity: 2
        };
      }
    }
    
    return null;
  }
  
  getGrade(score) {
    if (score >= 90) return 'A';
    if (score >= 80) return 'B';
    if (score >= 70) return 'C';
    if (score >= 60) return 'D';
    return 'F';
  }
  
  generateReport(dataArray, type = 'product') {
    const results = dataArray.map(item => ({
      data: item,
      quality: this.checkQuality(item, type)
    }));
    
    const summary = {
      total: results.length,
      acceptable: results.filter(r => r.quality.isAcceptable).length,
      averageScore: results.reduce((sum, r) => sum + r.quality.score, 0) / results.length,
      gradeDistribution: {}
    };
    
    // Grade distribution
    for (const result of results) {
      const grade = result.quality.grade;
      summary.gradeDistribution[grade] = (summary.gradeDistribution[grade] || 0) + 1;
    }
    
    // Common issues
    const issueTypes = {};
    for (const result of results) {
      for (const issue of result.quality.issues) {
        const key = `${issue.field}_${issue.type}`;
        issueTypes[key] = (issueTypes[key] || 0) + 1;
      }
    }
    
    return {
      summary,
      results,
      commonIssues: Object.entries(issueTypes)
        .sort((a, b) => b[1] - a[1])
        .slice(0, 10)
        .map(([key, count]) => ({ issue: key, count }))
    };
  }
}
```

---

## 16. Popüler Site Desenleri

### E-ticaret Siteleri

```javascript
// Amazon scraping patterns
class AmazonScraper {
  constructor(page) {
    this.page = page;
    this.selectors = {
      productList: '[data-component-type="s-search-result"]',
      title: 'h2 a span, [data-cy="title-recipe-title"]',
      price: '.a-price-whole, .a-offscreen',
      originalPrice: '.a-price.a-text-price .a-offscreen',
      rating: '.a-icon-alt',
      reviewCount: '.a-size-base',
      image: '.s-image',
      link: 'h2 a',
      prime: '[aria-label*="Prime"]',
      availability: '.a-size-base-plus',
      brand: '[data-cy="title-recipe-brand"]',
      nextPage: '.s-pagination-next'
    };
  }
  
  async scrapeProductList(url) {
    await this.page.goto(url, { waitUntil: 'networkidle0' });
    
    // CAPTCHA kontrolü
    if (await this.page.$('.a-box-inner.a-alert-container')) {
      console.log('Amazon CAPTCHA algılandı, manuel çözüm bekleniyor...');
      await this.page.waitForSelector(this.selectors.productList, { timeout: 60000 });
    }
    
    return await this.page.evaluate((selectors) => {
      const products = [];
      const productElements = document.querySelectorAll(selectors.productList);
      
      productElements.forEach(element => {
        const getTextContent = (selector) => {
          const el = element.querySelector(selector);
          return el ? el.textContent.trim() : null;
        };
        
        const getAttribute = (selector, attr) => {
          const el = element.querySelector(selector);
          return el ? el.getAttribute(attr) : null;
        };
        
        const title = getTextContent(selectors.title);
        if (!title) return;
        
        products.push({
          title,
          price: getTextContent(selectors.price),
          originalPrice: getTextContent(selectors.originalPrice),
          rating: getAttribute(selectors.rating, 'alt'),
          reviewCount: getTextContent(selectors.reviewCount),
          image: getAttribute(selectors.image, 'src'),
          link: getAttribute(selectors.link, 'href'),
          isPrime: !!element.querySelector(selectors.prime),
          availability: getTextContent(selectors.availability),
          brand: getTextContent(selectors.brand),
          source: 'amazon'
        });
      });
      
      return products;
    }, this.selectors);
  }
  
  async handleInfiniteScroll() {
    let previousProductCount = 0;
    let currentProductCount = 0;
    
    do {
      previousProductCount = currentProductCount;
      
      // Sayfanın sonuna scroll
      await this.page.evaluate(() => {
        window.scrollTo(0, document.body.scrollHeight);
      });
      
      // Yeni ürünlerin yüklenmesini bekle
      await this.page.waitForTimeout(2000);
      
      currentProductCount = await this.page.$$eval(
        this.selectors.productList,
        elements => elements.length
      );
    } while (currentProductCount > previousProductCount);
  }
}

// eBay patterns
class EbayScraper {
  constructor(page) {
    this.page = page;
    this.selectors = {
      productList: '.s-item',
      title: '.s-item__title',
      price: '.s-item__price',
      shipping: '.s-item__shipping',
      condition: '.s-item__subtitle',
      image: '.s-item__image img',
      link: '.s-item__link',
      watchCount: '.s-item__watchheart',
      location: '.s-item__location',
      seller: '.s-item__seller-info-text'
    };
  }
  
  async scrapeProductList(url) {
    await this.page.goto(url, { waitUntil: 'networkidle2' });
    
    return await this.page.evaluate((selectors) => {
      const products = [];
      const productElements = document.querySelectorAll(selectors.productList);
      
      productElements.forEach((element, index) => {
        // İlk element genellikle boş/placeholder
        if (index === 0) return;
        
        const getTextContent = (selector) => {
          const el = element.querySelector(selector);
          return el ? el.textContent.trim() : null;
        };
        
        const getAttribute = (selector, attr) => {
          const el = element.querySelector(selector);
          return el ? el.getAttribute(attr) : null;
        };
        
        const title = getTextContent(selectors.title);
        if (!title || title.includes('Shop on eBay')) return;
        
        products.push({
          title,
          price: getTextContent(selectors.price),
          shipping: getTextContent(selectors.shipping),
          condition: getTextContent(selectors.condition),
          image: getAttribute(selectors.image, 'src'),
          link: getAttribute(selectors.link, 'href'),
          watchCount: getTextContent(selectors.watchCount),
          location: getTextContent(selectors.location),
          seller: getTextContent(selectors.seller),
          source: 'ebay'
        });
      });
      
      return products;
    }, this.selectors);
  }
}
```

### Sosyal Medya Platformları

```javascript
// Instagram scraping (public posts only)
class InstagramScraper {
  constructor(page) {
    this.page = page;
    this.selectors = {
      posts: 'article',
      username: 'header a',
      caption: 'div[data-testid="post-caption"] span',
      likes: 'section button span',
      comments: 'section a span',
      timestamp: 'time',
      image: 'div img',
      video: 'video',
      hashtags: 'div[data-testid="post-caption"] a[href*="/explore/tags/"]',
      mentions: 'div[data-testid="post-caption"] a[href*="/"][role="link"]'
    };
  }
  
  async scrapePublicProfile(username) {
    const url = `https://instagram.com/${username}/`;
    await this.page.goto(url, { waitUntil: 'networkidle2' });
    
    // Private account check
    const isPrivate = await this.page.$('h2:contains("This Account is Private")');
    if (isPrivate) {
      throw new Error('Account is private');
    }
    
    // Load more posts by scrolling
    await this.loadMorePosts();
    
    return await this.extractPosts();
  }
  
  async loadMorePosts(maxScrolls = 5) {
    for (let i = 0; i < maxScrolls; i++) {
      await this.page.evaluate(() => {
        window.scrollTo(0, document.body.scrollHeight);
      });
      
      await this.page.waitForTimeout(2000);
      
      // Check if "Show more posts" button exists
      const showMoreButton = await this.page.$('button:contains("Show more posts")');
      if (showMoreButton) {
        await showMoreButton.click();
        await this.page.waitForTimeout(3000);
      }
    }
  }
  
  async extractPosts() {
    return await this.page.evaluate((selectors) => {
      const posts = [];
      const postElements = document.querySelectorAll(selectors.posts);
      
      postElements.forEach(element => {
        const getTextContent = (selector) => {
          const el = element.querySelector(selector);
          return el ? el.textContent.trim() : null;
        };
        
        const getAttribute = (selector, attr) => {
          const el = element.querySelector(selector);
          return el ? el.getAttribute(attr) : null;
        };
        
        const getAllText = (selector) => {
          const elements = element.querySelectorAll(selector);
          return Array.from(elements).map(el => el.textContent.trim());
        };
        
        posts.push({
          username: getTextContent(selectors.username),
          caption: getTextContent(selectors.caption),
          likes: getTextContent(selectors.likes),
          comments: getTextContent(selectors.comments),
          timestamp: getAttribute(selectors.timestamp, 'datetime'),
          image: getAttribute(selectors.image, 'src'),
          video: getAttribute(selectors.video, 'src'),
          hashtags: getAllText(selectors.hashtags),
          mentions: getAllText(selectors.mentions),
          source: 'instagram'
        });
      });
      
      return posts;
    }, this.selectors);
  }
}

// Twitter/X scraping patterns
class TwitterScraper {
  constructor(page) {
    this.page = page;
    this.selectors = {
      tweets: '[data-testid="tweet"]',
      username: '[data-testid="User-Name"] span',
      handle: '[data-testid="User-Name"] a',
      content: '[data-testid="tweetText"]',
      timestamp: 'time',
      likes: '[data-testid="like"] span',
      retweets: '[data-testid="retweet"] span',
      replies: '[data-testid="reply"] span',
      images: '[data-testid="tweetPhoto"] img',
      videos: 'video',
      links: '[data-testid="tweetText"] a',
      hashtags: '[data-testid="tweetText"] a[href*="/hashtag/"]',
      mentions: '[data-testid="tweetText"] a[href*="/"][dir="ltr"]'
    };
  }
  
  async scrapeUserTimeline(username, maxTweets = 20) {
    const url = `https://twitter.com/${username}`;
    await this.page.goto(url, { waitUntil: 'networkidle2' });
    
    // Login check
    if (this.page.url().includes('/login')) {
      throw new Error('Twitter login required');
    }
    
    const tweets = [];
    let previousTweetCount = 0;
    
    while (tweets.length < maxTweets) {
      // Scroll to load more tweets
      await this.page.evaluate(() => {
        window.scrollTo(0, document.body.scrollHeight);
      });
      
      await this.page.waitForTimeout(2000);
      
      const newTweets = await this.extractTweets();
      
      // Add only new tweets
      for (const tweet of newTweets) {
        if (!tweets.find(t => t.timestamp === tweet.timestamp && t.content === tweet.content)) {
          tweets.push(tweet);
        }
      }
      
      // Break if no new tweets loaded
      if (tweets.length === previousTweetCount) {
        break;
      }
      
      previousTweetCount = tweets.length;
    }
    
    return tweets.slice(0, maxTweets);
  }
  
  async extractTweets() {
    return await this.page.evaluate((selectors) => {
      const tweets = [];
      const tweetElements = document.querySelectorAll(selectors.tweets);
      
      tweetElements.forEach(element => {
        const getTextContent = (selector) => {
          const el = element.querySelector(selector);
          return el ? el.textContent.trim() : null;
        };
        
        const getAttribute = (selector, attr) => {
          const el = element.querySelector(selector);
          return el ? el.getAttribute(attr) : null;
        };
        
        const getAllAttributes = (selector, attr) => {
          const elements = element.querySelectorAll(selector);
          return Array.from(elements).map(el => el.getAttribute(attr));
        };
        
        const content = getTextContent(selectors.content);
        if (!content) return;
        
        tweets.push({
          username: getTextContent(selectors.username),
          handle: getAttribute(selectors.handle, 'href'),
          content: content,
          timestamp: getAttribute(selectors.timestamp, 'datetime'),
          likes: getTextContent(selectors.likes) || '0',
          retweets: getTextContent(selectors.retweets) || '0',
          replies: getTextContent(selectors.replies) || '0',
          images: getAllAttributes(selectors.images, 'src'),
          videos: getAllAttributes(selectors.videos, 'src'),
          links: getAllAttributes(selectors.links, 'href'),
          hashtags: getAllAttributes(selectors.hashtags, 'href'),
          mentions: getAllAttributes(selectors.mentions, 'href'),
          source: 'twitter'
        });
      });
      
      return tweets;
    }, this.selectors);
  }
}
```

### Haber ve İçerik Siteleri

```javascript
class NewsScraper {
  constructor(page) {
    this.page = page;
    this.commonSelectors = {
      article: 'article, .article, .post, .entry',
      title: 'h1, .title, .headline, [class*="title"]',
      content: '.content, .article-body, .post-content, .entry-content, p',
      author: '.author, .byline, [rel="author"]',
      date: 'time, .date, .published, [datetime]',
      tags: '.tags a, .categories a, .tag',
      image: '.featured-image img, .article-image img, .hero-image img'
    };
  }
  
  async scrapeArticle(url) {
    await this.page.goto(url, { waitUntil: 'networkidle2' });
    
    // Try to detect article structure
    const article = await this.detectArticleStructure();
    
    if (!article) {
      throw new Error('Article structure not detected');
    }
    
    return article;
  }
  
  async detectArticleStructure() {
    // Try JSON-LD structured data first
    let article = await this.extractJSONLD();
    if (article) return article;
    
    // Try Open Graph tags
    article = await this.extractOpenGraph();
    if (article) return article;
    
    // Fallback to common selectors
    return await this.extractCommonStructure();
  }
  
  async extractJSONLD() {
    return await this.page.evaluate(() => {
      const scripts = document.querySelectorAll('script[type="application/ld+json"]');
      
      for (const script of scripts) {
        try {
          const data = JSON.parse(script.textContent);
          
          if (data['@type'] === 'Article' || data['@type'] === 'NewsArticle') {
            return {
              title: data.headline,
              content: data.articleBody,
              author: data.author?.name || data.author,
              date: data.datePublished,
              image: data.image?.url || data.image,
              publisher: data.publisher?.name,
              source: 'json-ld'
            };
          }
        } catch (error) {
          continue;
        }
      }
      
      return null;
    });
  }
  
  async extractOpenGraph() {
    return await this.page.evaluate(() => {
      const getMetaContent = (property) => {
        const meta = document.querySelector(`meta[property="og:${property}"]`);
        return meta ? meta.content : null;
      };
      
      const title = getMetaContent('title');
      if (!title) return null;
      
      return {
        title,
        content: getMetaContent('description'),
        image: getMetaContent('image'),
        url: getMetaContent('url'),
        type: getMetaContent('type'),
        siteName: getMetaContent('site_name'),
        source: 'open-graph'
      };
    });
  }
  
  async extractCommonStructure() {
    return await this.page.evaluate((selectors) => {
      const getTextContent = (selector) => {
        const el = document.querySelector(selector);
        return el ? el.textContent.trim() : null;
      };
      
      const getAttribute = (selector, attr) => {
        const el = document.querySelector(selector);
        return el ? el.getAttribute(attr) : null;
      };
      
      const getAllText = (selector) => {
        const elements = document.querySelectorAll(selector);
        return Array.from(elements).map(el => el.textContent.trim());
      };
      
      // Content extraction with multiple paragraph support
      const contentElements = document.querySelectorAll(selectors.content);
      const content = Array.from(contentElements)
        .map(el => el.textContent.trim())
        .filter(text => text.length > 50)
        .join('\n\n');
      
      return {
        title: getTextContent(selectors.title),
        content: content || getTextContent(selectors.content),
        author: getTextContent(selectors.author),
        date: getAttribute(selectors.date, 'datetime') || getTextContent(selectors.date),
        tags: getAllText(selectors.tags),
        image: getAttribute(selectors.image, 'src'),
        source: 'common-structure'
      };
    }, this.commonSelectors);
  }
}
```

---

## 17. Webhook ve Bildirimler

### Slack Entegrasyonu

```javascript
import axios from 'axios';

class SlackNotifier {
  constructor(webhookUrl, defaultChannel = '#scraping') {
    this.webhookUrl = webhookUrl;
    this.defaultChannel = defaultChannel;
  }
  
  async sendMessage(message, options = {}) {
    const payload = {
      channel: options.channel || this.defaultChannel,
      username: options.username || 'Scraping Bot',
      icon_emoji: options.emoji || ':robot_face:',
      text: message,
      ...options
    };
    
    try {
      await axios.post(this.webhookUrl, payload);
    } catch (error) {
      console.error('Slack notification error:', error.message);
    }
  }
  
  async sendRichMessage(title, fields, options = {}) {
    const attachment = {
      color: options.color || 'good',
      title: title,
      fields: fields.map(field => ({
        title: field.title,
        value: field.value,
        short: field.short || false
      })),
      footer: 'Scraping Bot',
      ts: Math.floor(Date.now() / 1000)
    };
    
    const payload = {
      channel: options.channel || this.defaultChannel,
      username: options.username || 'Scraping Bot',
      attachments: [attachment]
    };
    
    try {
      await axios.post(this.webhookUrl, payload);
    } catch (error) {
      console.error('Slack rich message error:', error.message);
    }
  }
  
  async notifyScrapingStart(jobId, urls) {
    const fields = [
      { title: 'Job ID', value: jobId, short: true },
      { title: 'URLs Count', value: urls.length.toString(), short: true },
      { title: 'URLs', value: urls.slice(0, 5).join('\n') + (urls.length > 5 ? '\n...' : ''), short: false }
    ];
    
    await this.sendRichMessage(
      '🚀 Scraping Job Started',
      fields,
      { color: '#36a64f' }
    );
  }
  
  async notifyScrapingComplete(jobId, stats) {
    const fields = [
      { title: 'Job ID', value: jobId, short: true },
      { title: 'Success Rate', value: `${stats.successRate}%`, short: true },
      { title: 'Total Processed', value: stats.totalProcessed.toString(), short: true },
      { title: 'Duration', value: this.formatDuration(stats.duration), short: true },
      { title: 'Items Scraped', value: stats.itemsScraped.toString(), short: true },
      { title: 'Errors', value: stats.errors.toString(), short: true }
    ];
    
    const color = stats.successRate >= 80 ? '#36a64f' : 
                  stats.successRate >= 60 ? '#ffa500' : '#ff0000';
    
    await this.sendRichMessage(
      '✅ Scraping Job Completed',
      fields,
      { color }
    );
  }
  
  async notifyError(jobId, error, context = {}) {
    const fields = [
      { title: 'Job ID', value: jobId, short: true },
      { title: 'Error Type', value: error.name || 'Unknown', short: true },
      { title: 'Message', value: error.message, short: false }
    ];
    
    if (context.url) {
      fields.push({ title: 'URL', value: context.url, short: false });
    }
    
    if (context.stack) {
      fields.push({ 
        title: 'Stack Trace', 
        value: context.stack.substring(0, 500) + '...', 
        short: false 
      });
    }
    
    await this.sendRichMessage(
      '❌ Scraping Error',
      fields,
      { color: '#ff0000' }
    );
  }
  
  formatDuration(ms) {
    const seconds = Math.floor(ms / 1000);
    const minutes = Math.floor(seconds / 60);
    const hours = Math.floor(minutes / 60);
    
    if (hours > 0) return `${hours}h ${minutes % 60}m`;
    if (minutes > 0) return `${minutes}m ${seconds % 60}s`;
    return `${seconds}s`;
  }
}
```

### Discord Entegrasyonu

```javascript
class DiscordNotifier {
  constructor(webhookUrl) {
    this.webhookUrl = webhookUrl;
  }
  
  async sendEmbed(embed, content = '') {
    const payload = {
      content,
      embeds: [embed]
    };
    
    try {
      await axios.post(this.webhookUrl, payload);
    } catch (error) {
      console.error('Discord notification error:', error.message);
    }
  }
  
  async notifyScrapingProgress(jobId, progress) {
    const embed = {
      title: '⏳ Scraping Progress',
      color: 0x3498db,
      fields: [
        { name: 'Job ID', value: jobId, inline: true },
        { name: 'Progress', value: `${progress.percentage}%`, inline: true },
        { name: 'Processed', value: `${progress.processed}/${progress.total}`, inline: true },
        { name: 'Success Rate', value: `${progress.successRate}%`, inline: true },
        { name: 'ETA', value: progress.eta, inline: true },
        { name: 'Speed', value: `${progress.itemsPerSecond}/sec`, inline: true }
      ],
      timestamp: new Date().toISOString(),
      footer: { text: 'Scraping Bot' }
    };
    
    await this.sendEmbed(embed);
  }
  
  async notifyDataQuality(jobId, qualityReport) {
    const color = qualityReport.summary.averageScore >= 80 ? 0x2ecc71 : 
                  qualityReport.summary.averageScore >= 60 ? 0xf39c12 : 0xe74c3c;
    
    const embed = {
      title: '📊 Data Quality Report',
      color,
      fields: [
        { name: 'Job ID', value: jobId, inline: true },
        { name: 'Average Score', value: `${qualityReport.summary.averageScore.toFixed(2)}/100`, inline: true },
        { name: 'Acceptable Items', value: `${qualityReport.summary.acceptable}/${qualityReport.summary.total}`, inline: true }
      ],
      timestamp: new Date().toISOString()
    };
    
    // Add top issues
    if (qualityReport.commonIssues.length > 0) {
      const issuesText = qualityReport.commonIssues
        .slice(0, 5)
        .map(issue => `${issue.issue}: ${issue.count}`)
        .join('\n');
      
      embed.fields.push({
        name: 'Common Issues',
        value: issuesText,
        inline: false
      });
    }
    
    await this.sendEmbed(embed);
  }
}
```

### Email Bildirimler

```javascript
import nodemailer from 'nodemailer';
import fs from 'fs/promises';

class EmailNotifier {
  constructor(config) {
    this.transporter = nodemailer.createTransporter({
      host: config.host,
      port: config.port,
      secure: config.secure || false,
      auth: {
        user: config.username,
        pass: config.password
      }
    });
    
    this.fromEmail = config.fromEmail || config.username;
    this.templates = new Map();
  }
  
  async loadTemplate(templateName) {
    if (this.templates.has(templateName)) {
      return this.templates.get(templateName);
    }
    
    try {
      const template = await fs.readFile(`templates/${templateName}.html`, 'utf8');
      this.templates.set(templateName, template);
      return template;
    } catch (error) {
      console.error(`Template ${templateName} not found:`, error.message);
      return null;
    }
  }
  
  async sendScrapingReport(to, jobId, stats, data = []) {
    const template = await this.loadTemplate('scraping-report');
    
    if (!template) {
      // Fallback to plain text
      const subject = `Scraping Report - Job ${jobId}`;
      const text = this.generatePlainTextReport(stats, data);
      
      return await this.sendEmail(to, subject, text);
    }
    
    // Use HTML template
    const html = this.fillTemplate(template, {
      jobId,
      stats,
      data: data.slice(0, 10), // Show first 10 items
      timestamp: new Date().toLocaleString()
    });
    
    const mailOptions = {
      from: this.fromEmail,
      to,
      subject: `Scraping Report - Job ${jobId}`,
      html,
      attachments: [{
        filename: `scraping-data-${jobId}.json`,
        content: JSON.stringify(data, null, 2),
        contentType: 'application/json'
      }]
    };
    
    try {
      await this.transporter.sendMail(mailOptions);
      console.log('Scraping report sent successfully');
    } catch (error) {
      console.error('Email sending failed:', error.message);
    }
  }
  
  async sendErrorAlert(to, jobId, error, context = {}) {
    const subject = `🚨 Scraping Error Alert - Job ${jobId}`;
    
    const html = `
      <h2>Scraping Error Alert</h2>
      <p><strong>Job ID:</strong> ${jobId}</p>
      <p><strong>Error:</strong> ${error.message}</p>
      <p><strong>Time:</strong> ${new Date().toLocaleString()}</p>
      
      ${context.url ? `<p><strong>URL:</strong> ${context.url}</p>` : ''}
      
      ${error.stack ? `
        <h3>Stack Trace:</h3>
        <pre style="background: #f5f5f5; padding: 10px; overflow: auto;">
          ${error.stack}
        </pre>
      ` : ''}
      
      <h3>Context:</h3>
      <pre style="background: #f5f5f5; padding: 10px;">
        ${JSON.stringify(context, null, 2)}
      </pre>
    `;
    
    await this.sendEmail(to, subject, '', html);
  }
  
  async sendEmail(to, subject, text, html = '') {
    const mailOptions = {
      from: this.fromEmail,
      to: Array.isArray(to) ? to.join(', ') : to,
      subject,
      text,
      html: html || text
    };
    
    try {
      const info = await this.transporter.sendMail(mailOptions);
      console.log('Email sent:', info.messageId);
      return true;
    } catch (error) {
      console.error('Email error:', error.message);
      return false;
    }
  }
  
  fillTemplate(template, data) {
    return template.replace(/\{\{(\w+)\}\}/g, (match, key) => {
      if (key in data) {
        if (typeof data[key] === 'object') {
          return JSON.stringify(data[key], null, 2);
        }
        return data[key].toString();
      }
      return match;
    });
  }
  
  generatePlainTextReport(stats, data) {
    return `
Scraping Job Report
==================

Statistics:
- Total Processed: ${stats.totalProcessed}
- Success Rate: ${stats.successRate}%
- Items Scraped: ${stats.itemsScraped}
- Duration: ${stats.duration}ms
- Errors: ${stats.errors}

Sample Data:
${data.slice(0, 5).map(item => `- ${item.title || item.name || JSON.stringify(item).substring(0, 100)}`).join('\n')}

Generated at: ${new Date().toLocaleString()}
    `;
  }
}
```

### Webhook Yönetim Sistemi

```javascript
class WebhookManager {
  constructor() {
    this.webhooks = new Map();
    this.eventQueue = [];
    this.processing = false;
  }
  
  registerWebhook(name, url, events = []) {
    this.webhooks.set(name, {
      url,
      events,
      active: true,
      lastUsed: null,
      failureCount: 0,
      maxFailures: 3
    });
  }
  
  async triggerEvent(eventType, data) {
    const event = {
      type: eventType,
      data,
      timestamp: Date.now(),
      id: this.generateEventId()
    };
    
    this.eventQueue.push(event);
    
    if (!this.processing) {
      this.processQueue();
    }
  }
  
  async processQueue() {
    this.processing = true;
    
    while (this.eventQueue.length > 0) {
      const event = this.eventQueue.shift();
      await this.processEvent(event);
    }
    
    this.processing = false;
  }
  
  async processEvent(event) {
    const relevantWebhooks = Array.from(this.webhooks.entries())
      .filter(([name, webhook]) => 
        webhook.active && 
        (webhook.events.length === 0 || webhook.events.includes(event.type))
      );
    
    const promises = relevantWebhooks.map(([name, webhook]) => 
      this.callWebhook(name, webhook, event)
    );
    
    await Promise.allSettled(promises);
  }
  
  async callWebhook(name, webhook, event) {
    const payload = {
      event: event.type,
      timestamp: event.timestamp,
      id: event.id,
      data: event.data
    };
    
    try {
      const response = await axios.post(webhook.url, payload, {
        timeout: 10000,
        headers: {
          'Content-Type': 'application/json',
          'User-Agent': 'ScrapingBot-Webhook/1.0'
        }
      });
      
      if (response.status >= 200 && response.status < 300) {
        webhook.lastUsed = Date.now();
        webhook.failureCount = 0;
        console.log(`Webhook ${name} called successfully`);
      } else {
        throw new Error(`HTTP ${response.status}`);
      }
    } catch (error) {
      webhook.failureCount++;
      console.error(`Webhook ${name} failed:`, error.message);
      
      if (webhook.failureCount >= webhook.maxFailures) {
        webhook.active = false;
        console.warn(`Webhook ${name} disabled due to repeated failures`);
      }
    }
  }
  
  generateEventId() {
    return Date.now().toString(36) + Math.random().toString(36).substring(2);
  }
  
  getWebhookStatus() {
    const status = {};
    
    for (const [name, webhook] of this.webhooks) {
      status[name] = {
        active: webhook.active,
        lastUsed: webhook.lastUsed,
        failureCount: webhook.failureCount,
        events: webhook.events
      };
    }
    
    return status;
  }
}
```

---

## 18. Docker ve Containerization

### Dockerfile

```dockerfile
# Multi-stage build for production optimization
FROM node:18-alpine AS base

# Install system dependencies
RUN apk add --no-cache \
    chromium \
    nss \
    freetype \
    harfbuzz \
    ca-certificates \
    ttf-freefont \
    && rm -rf /var/cache/apk/*

# Tell Puppeteer to skip installing Chrome (we just installed it)
ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true
ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser

# Create app directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
FROM base AS dependencies
RUN npm ci --only=production && npm cache clean --force

# Development stage
FROM base AS development
RUN npm ci && npm cache clean --force
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]

# Build stage
FROM dependencies AS build
COPY . .
RUN npm run build

# Production stage
FROM base AS production

# Copy built application and dependencies
COPY --from=dependencies /app/node_modules ./node_modules
COPY --from=build /app/dist ./dist
COPY --from=build /app/config ./config
COPY --from=build /app/templates ./templates

# Create non-root user
RUN addgroup -g 1001 -S nodejs
RUN adduser -S puppeteer -u 1001

# Change ownership of app directory
RUN chown -R puppeteer:nodejs /app
USER puppeteer

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD node healthcheck.js

EXPOSE 3000

CMD ["node", "dist/index.js"]
```

### Docker Compose

```yaml
version: '3.8'

services:
  # Main scraping application
  scraper:
    build:
      context: .
      target: production
      args:
        NODE_ENV: production
    environment:
      - NODE_ENV=production
      - DB_HOST=postgres
      - DB_NAME=scraping
      - DB_USER=scraper
      - DB_PASS=scraperpass123
      - REDIS_HOST=redis
      - LOG_LEVEL=info
      - MAX_CONCURRENCY=3
      - HEADLESS_MODE=true
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
    volumes:
      - ./data:/app/data
      - ./logs:/app/logs
      - ./config:/app/config:ro
    networks:
      - scraper-network
    restart: unless-stopped
    deploy:
      replicas: 2
      resources:
        limits:
          memory: 2G
          cpus: '1.0'
        reservations:
          memory: 1G
          cpus: '0.5'

  # Worker instances for scaling
  scraper-worker:
    build:
      context: .
      target: production
    environment:
      - NODE_ENV=production
      - WORKER_MODE=true
      - DB_HOST=postgres
      - DB_NAME=scraping
      - DB_USER=scraper
      - DB_PASS=scraperpass123
      - REDIS_HOST=redis
    depends_on:
      - postgres
      - redis
    volumes:
      - ./data:/app/data
      - ./logs:/app/logs
    networks:
      - scraper-network
    restart: unless-stopped
    deploy:
      replicas: 3

  # PostgreSQL Database
  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: scraping
      POSTGRES_USER: scraper
      POSTGRES_PASSWORD: scraperpass123
      POSTGRES_INITDB_ARGS: "--encoding=UTF-8"
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./db/init:/docker-entrypoint-initdb.d:ro
    networks:
      - scraper-network
    restart: unless-stopped
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U scraper -d scraping"]
      interval: 10s
      timeout: 5s
      retries: 5
    deploy:
      resources:
        limits:
          memory: 1G
          cpus: '0.5'

  # Redis for caching and job queues
  redis:
    image: redis:7-alpine
    command: redis-server --appendonly yes --requirepass redispass123
    environment:
      REDIS_PASSWORD: redispass123
    volumes:
      - redis_data:/data
    networks:
      - scraper-network
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "redis-cli", "--raw", "incr", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Monitoring with Prometheus
  prometheus:
    image: prom/prometheus:latest
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.console.libraries=/etc/prometheus/console_libraries'
      - '--web.console.templates=/etc/prometheus/consoles'
    volumes:
      - ./monitoring/prometheus.yml:/etc/prometheus/prometheus.yml:ro
      - prometheus_data:/prometheus
    networks:
      - scraper-network
    ports:
      - "9090:9090"
    restart: unless-stopped

  # Grafana for visualization
  grafana:
    image: grafana/grafana:latest
    environment:
      GF_SECURITY_ADMIN_PASSWORD: grafanapass123
      GF_USERS_ALLOW_SIGN_UP: false
    volumes:
      - grafana_data:/var/lib/grafana
      - ./monitoring/grafana/dashboards:/etc/grafana/provisioning/dashboards:ro
      - ./monitoring/grafana/datasources:/etc/grafana/provisioning/datasources:ro
    networks:
      - scraper-network
    ports:
      - "3001:3000"
    restart: unless-stopped
    depends_on:
      - prometheus

  # Nginx reverse proxy
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/ssl:/etc/nginx/ssl:ro
    networks:
      - scraper-network
    restart: unless-stopped
    depends_on:
      - scraper

volumes:
  postgres_data:
  redis_data:
  prometheus_data:
  grafana_data:

networks:
  scraper-network:
    driver: bridge
```

### Docker Swarm Deployment

```yaml
# docker-swarm-stack.yml
version: '3.8'

services:
  scraper:
    image: scraper:latest
    environment:
      - NODE_ENV=production
    secrets:
      - db_password
      - redis_password
      - api_keys
    configs:
      - source: scraper_config
        target: /app/config/production.json
    deploy:
      replicas: 3
      placement:
        constraints:
          - node.role == worker
      resources:
        limits:
          memory: 2G
          cpus: '1.0'
        reservations:
          memory: 1G
          cpus: '0.5'
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
      update_config:
        parallelism: 1
        delay: 10s
        failure_action: rollback
    healthcheck:
      test: ["CMD", "node", "healthcheck.js"]
      interval: 30s
      timeout: 10s
      retries: 3
    networks:
      - scraper-overlay

  postgres:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
    secrets:
      - db_password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    deploy:
      replicas: 1
      placement:
        constraints:
          - node.role == manager
    networks:
      - scraper-overlay

secrets:
  db_password:
    external: true
  redis_password:
    external: true
  api_keys:
    external: true

configs:
  scraper_config:
    external: true

volumes:
  postgres_data:

networks:
  scraper-overlay:
    driver: overlay
    attachable: true
```

### Kubernetes Deployment

```yaml
# k8s-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: scraper-app
  labels:
    app: scraper
spec:
  replicas: 3
  selector:
    matchLabels:
      app: scraper
  template:
    metadata:
      labels:
        app: scraper
    spec:
      containers:
      - name: scraper
        image: scraper:latest
        ports:
        - containerPort: 3000
        env:
        - name: NODE_ENV
          value: "production"
        - name: DB_HOST
          value: "postgres-service"
        - name: REDIS_HOST
          value: "redis-service"
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: password
        resources:
          limits:
            memory: "2Gi"
            cpu: "1000m"
          requests:
            memory: "1Gi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 5
        volumeMounts:
        - name: config-volume
          mountPath: /app/config
        - name: data-volume
          mountPath: /app/data
      volumes:
      - name: config-volume
        configMap:
          name: scraper-config
      - name: data-volume
        persistentVolumeClaim:
          claimName: scraper-pvc
---
apiVersion: v1
kind: Service
metadata:
  name: scraper-service
spec:
  selector:
    app: scraper
  ports:
  - port: 80
    targetPort: 3000
  type: LoadBalancer
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: scraper-config
data:
  production.json: |
    {
      "database": {
        "host": "postgres-service",
        "port": 5432,
        "database": "scraping"
      },
      "redis": {
        "host": "redis-service",
        "port": 6379
      },
      "scraping": {
        "maxConcurrency": 3,
        "headless": true
      }
    }
---
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  password: c2NyYXBlcnBhc3MxMjM= # base64 encoded
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: scraper-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

### Health Check Implementation

```javascript
// healthcheck.js
import http from 'http';
import { getConfig } from './config/index.js';
import { MongoDBManager } from './database/mongodb.js';
import { RedisCache } from './cache/redis.js';

class HealthCheck {
  constructor() {
    this.config = getConfig();
    this.checks = new Map();
  }
  
  async runChecks() {
    const results = {
      status: 'healthy',
      timestamp: new Date().toISOString(),
      checks: {}
    };
    
    // Database check
    try {
      const db = new MongoDBManager(this.config.database.connectionString);
      await db.connect();
      await db.disconnect();
      results.checks.database = { status: 'healthy' };
    } catch (error) {
      results.checks.database = { 
        status: 'unhealthy', 
        error: error.message 
      };
      results.status = 'unhealthy';
    }
    
    // Redis check
    try {
      const redis = new RedisCache(this.config.redis);
      await redis.set('health_check', 'ok', 10);
      const value = await redis.get('health_check');
      if (value !== 'ok') throw new Error('Redis read/write failed');
      results.checks.redis = { status: 'healthy' };
    } catch (error) {
      results.checks.redis = { 
        status: 'unhealthy', 
        error: error.message 
      };
      results.status = 'unhealthy';
    }
    
    // Memory usage check
    const memUsage = process.memoryUsage();
    const memUsageMB = Math.round(memUsage.heapUsed / 1024 / 1024);
    const memLimit = 1500; // MB
    
    results.checks.memory = {
      status: memUsageMB < memLimit ? 'healthy' : 'warning',
      usage: `${memUsageMB}MB`,
      limit: `${memLimit}MB`
    };
    
    if (memUsageMB > memLimit * 1.5) {
      results.status = 'unhealthy';
    }
    
    return results;
  }
}

// CLI usage for Docker health check
if (process.argv[2] === '--cli') {
  const healthCheck = new HealthCheck();
  
  healthCheck.runChecks()
    .then(results => {
      console.log(JSON.stringify(results, null, 2));
      process.exit(results.status === 'healthy' ? 0 : 1);
    })
    .catch(error => {
      console.error('Health check failed:', error);
      process.exit(1);
    });
}

// HTTP server for Kubernetes health checks
const server = http.createServer(async (req, res) => {
  if (req.url === '/health' || req.url === '/healthz') {
    const healthCheck = new HealthCheck();
    
    try {
      const results = await healthCheck.runChecks();
      
      res.writeHead(results.status === 'healthy' ? 200 : 503, {
        'Content-Type': 'application/json'
      });
      res.end(JSON.stringify(results));
    } catch (error) {
      res.writeHead(503, { 'Content-Type': 'application/json' });
      res.end(JSON.stringify({
        status: 'unhealthy',
        error: error.message,
        timestamp: new Date().toISOString()
      }));
    }
  } else {
    res.writeHead(404);
    res.end('Not found');
  }
});

const port = process.env.HEALTH_PORT || 3001;
server.listen(port, () => {
  console.log(`Health check server running on port ${port}`);
});

export default HealthCheck;
```

---

## 19. CI/CD ve Production Deployment

### GitHub Actions Workflow

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

env:
  NODE_VERSION: '18'
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: scraping_test
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432
      
      redis:
        image: redis:7-alpine
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 6379:6379

    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: ${{ env.NODE_VERSION }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run linting
      run: npm run lint
    
    - name: Run type checking
      run: npm run type-check
    
    - name: Run tests
      env:
        NODE_ENV: test
        DB_HOST: localhost
        DB_PORT: 5432
        DB_NAME: scraping_test
        DB_USER: postgres
        DB_PASS: postgres
        REDIS_HOST: localhost
        REDIS_PORT: 6379
      run: npm test
    
    - name: Run integration tests
      env:
        NODE_ENV: test
        DB_HOST: localhost
        REDIS_HOST: localhost
      run: npm run test:integration
    
    - name: Generate test coverage
      run: npm run test:coverage
    
    - name: Upload coverage to Codecov
      uses: codecov/codecov-action@v3
      with:
        token: ${{ secrets.CODECOV_TOKEN }}
        file: ./coverage/lcov.info

  security:
    runs-on: ubuntu-latest
    needs: test
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: ${{ env.NODE_VERSION }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run security audit
      run: npm audit --audit-level high
    
    - name: Run Snyk security scan
      uses: snyk/actions/node@master
      env:
        SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
      with:
        args: --severity-threshold=high
    
    - name: Run CodeQL analysis
      uses: github/codeql-action/analyze@v2
      with:
        languages: javascript

  build:
    runs-on: ubuntu-latest
    needs: [test, security]
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Docker Buildx
      uses: docker/setup-buildx-action@v3
    
    - name: Login to Container Registry
      uses: docker/login-action@v3
      with:
        registry: ${{ env.REGISTRY }}
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}
    
    - name: Extract metadata
      id: meta
      uses: docker/metadata-action@v5
      with:
        images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
        tags: |
          type=ref,event=branch
          type=ref,event=pr
          type=sha,prefix={{branch}}-
          type=raw,value=latest,enable={{is_default_branch}}
    
    - name: Build and push Docker image
      uses: docker/build-push-action@v5
      with:
        context: .
        push: true
        target: production
        tags: ${{ steps.meta.outputs.tags }}
        labels: ${{ steps.meta.outputs.labels }}
        cache-from: type=gha
        cache-to: type=gha,mode=max
        platforms: linux/amd64,linux/arm64

  deploy-staging:
    runs-on: ubuntu-latest
    needs: build
    environment: staging
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Deploy to staging
      uses: appleboy/ssh-action@v1.0.0
      with:
        host: ${{ secrets.STAGING_HOST }}
        username: ${{ secrets.STAGING_USER }}
        key: ${{ secrets.STAGING_SSH_KEY }}
        script: |
          cd /opt/scraper
          docker-compose pull
          docker-compose up -d
          docker system prune -f
    
    - name: Run smoke tests
      run: |
        sleep 30
        curl -f http://${{ secrets.STAGING_HOST }}/health || exit 1
        npm run test:smoke -- --host ${{ secrets.STAGING_HOST }}

  deploy-production:
    runs-on: ubuntu-latest
    needs: deploy-staging
    environment: production
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup kubectl
      uses: azure/setup-kubectl@v3
      with:
        version: 'v1.28.0'
    
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1
    
    - name: Update kubeconfig
      run: aws eks update-kubeconfig --name production-cluster
    
    - name: Deploy to EKS
      run: |
        envsubst < k8s/deployment.yml | kubectl apply -f -
        kubectl rollout status deployment/scraper-app
        kubectl get services
      env:
        IMAGE_TAG: ${{ github.sha }}
    
    - name: Verify deployment
      run: |
        kubectl get pods -l app=scraper
        kubectl logs -l app=scraper --tail=50
    
    - name: Run production smoke tests
      run: npm run test:smoke -- --production

  notify:
    runs-on: ubuntu-latest
    needs: [deploy-production]
    if: always()
    
    steps:
    - name: Notify Slack
      uses: 8398a7/action-slack@v3
      with:
        status: ${{ job.status }}
        channel: '#deployments'
        webhook_url: ${{ secrets.SLACK_WEBHOOK }}
        fields: repo,message,commit,author,action,eventName,ref,workflow
```

### Terraform Infrastructure

```hcl
# infrastructure/main.tf
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  
  backend "s3" {
    bucket         = "scraper-terraform-state"
    key            = "production/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}

provider "aws" {
  region = var.aws_region
}

# Variables
variable "aws_region" {
  description = "AWS region"
  type        = string
  default     = "us-east-1"
}

variable "environment" {
  description = "Environment name"
  type        = string
  default     = "production"
}

variable "cluster_name" {
  description = "EKS cluster name"
  type        = string
  default     = "scraper-cluster"
}

# VPC Configuration
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  
  name = "${var.environment}-vpc"
  cidr = "10.0.0.0/16"
  
  azs             = ["${var.aws_region}a", "${var.aws_region}b", "${var.aws_region}c"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]
  
  enable_nat_gateway = true
  enable_vpn_gateway = true
  
  tags = {
    Environment = var.environment
    Project     = "scraper"
  }
}

# EKS Cluster
module "eks" {
  source = "terraform-aws-modules/eks/aws"
  
  cluster_name    = var.cluster_name
  cluster_version = "1.28"
  
  vpc_id     = module.vpc.vpc_id
  subnet_ids = module.vpc.private_subnets
  
  eks_managed_node_groups = {
    main = {
      desired_capacity = 3
      max_capacity     = 10
      min_capacity     = 1
      
      instance_types = ["t3.medium"]
      
      k8s_labels = {
        Environment = var.environment
        NodeGroup   = "main"
      }
    }
  }
  
  tags = {
    Environment = var.environment
    Project     = "scraper"
  }
}

# RDS PostgreSQL
resource "aws_db_instance" "postgres" {
  identifier = "${var.environment}-scraper-db"
  
  engine         = "postgres"
  engine_version = "15.3"
  instance_class = "db.t3.micro"
  
  allocated_storage     = 20
  max_allocated_storage = 100
  storage_encrypted     = true
  
  db_name  = "scraping"
  username = "scraper"
  password = var.db_password
  
  vpc_security_group_ids = [aws_security_group.rds.id]
  db_subnet_group_name   = aws_db_subnet_group.main.name
  
  backup_retention_period = 7
  backup_window          = "03:00-04:00"
  maintenance_window     = "sun:04:00-sun:05:00"
  
  skip_final_snapshot = true
  deletion_protection = false
  
  tags = {
    Environment = var.environment
    Project     = "scraper"
  }
}

# ElastiCache Redis
resource "aws_elasticache_subnet_group" "main" {
  name       = "${var.environment}-cache-subnet"
  subnet_ids = module.vpc.private_subnets
}

resource "aws_elasticache_cluster" "redis" {
  cluster_id           = "${var.environment}-scraper-redis"
  engine               = "redis"
  node_type            = "cache.t3.micro"
  num_cache_nodes      = 1
  parameter_group_name = "default.redis7"
  port                 = 6379
  subnet_group_name    = aws_elasticache_subnet_group.main.name
  security_group_ids   = [aws_security_group.redis.id]
  
  tags = {
    Environment = var.environment
    Project     = "scraper"
  }
}

# Security Groups
resource "aws_security_group" "rds" {
  name_prefix = "${var.environment}-rds-"
  vpc_id      = module.vpc.vpc_id
  
  ingress {
    from_port   = 5432
    to_port     = 5432
    protocol    = "tcp"
    cidr_blocks = [module.vpc.vpc_cidr_block]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = {
    Name        = "${var.environment}-rds-sg"
    Environment = var.environment
  }
}

resource "aws_security_group" "redis" {
  name_prefix = "${var.environment}-redis-"
  vpc_id      = module.vpc.vpc_id
  
  ingress {
    from_port   = 6379
    to_port     = 6379
    protocol    = "tcp"
    cidr_blocks = [module.vpc.vpc_cidr_block]
  }
  
  tags = {
    Name        = "${var.environment}-redis-sg"
    Environment = var.environment
  }
}

# S3 Bucket for data storage
resource "aws_s3_bucket" "data" {
  bucket = "${var.environment}-scraper-data-${random_id.bucket_suffix.hex}"
  
  tags = {
    Environment = var.environment
    Project     = "scraper"
  }
}

resource "aws_s3_bucket_versioning" "data" {
  bucket = aws_s3_bucket.data.id
  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_encryption" "data" {
  bucket = aws_s3_bucket.data.id
  
  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
      }
    }
  }
}

resource "random_id" "bucket_suffix" {
  byte_length = 8
}

# CloudWatch Log Groups
resource "aws_cloudwatch_log_group" "app" {
  name              = "/aws/eks/${var.cluster_name}/scraper"
  retention_in_days = 14
  
  tags = {
    Environment = var.environment
    Project     = "scraper"
  }
}

# Outputs
output "cluster_endpoint" {
  description = "EKS cluster endpoint"
  value       = module.eks.cluster_endpoint
}

output "cluster_name" {
  description = "EKS cluster name"
  value       = module.eks.cluster_id
}

output "database_endpoint" {
  description = "RDS instance endpoint"
  value       = aws_db_instance.postgres.endpoint
  sensitive   = true
}

output "redis_endpoint" {
  description = "Redis cluster endpoint"
  value       = aws_elasticache_cluster.redis.cache_nodes[0].address
  sensitive   = true
}
```

### Ansible Deployment

```yaml
# ansible/deploy.yml
---
- name: Deploy Scraper Application
  hosts: scraper_servers
  become: yes
  vars:
    app_name: scraper
    app_user: scraper
    app_dir: /opt/{{ app_name }}
    docker_compose_version: "2.23.0"
    
  tasks:
    - name: Update system packages
      apt:
        update_cache: yes
        upgrade: dist
        autoremove: yes
    
    - name: Install required packages
      apt:
        name:
          - curl
          - ca-certificates
          - gnupg
          - lsb-release
          - python3-pip
        state: present
    
    - name: Add Docker GPG key
      apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present
    
    - name: Add Docker repository
      apt_repository:
        repo: "deb [arch=amd64] https://download.docker.com/linux/ubuntu {{ ansible_distribution_release }} stable"
        state: present
    
    - name: Install Docker
      apt:
        name:
          - docker-ce
          - docker-ce-cli
          - containerd.io
        state: present
    
    - name: Install Docker Compose
      pip:
        name: docker-compose
        state: present
    
    - name: Create application user
      user:
        name: "{{ app_user }}"
        system: yes
        shell: /bin/bash
        home: "{{ app_dir }}"
        create_home: yes
    
    - name: Add user to docker group
      user:
        name: "{{ app_user }}"
        groups: docker
        append: yes
    
    - name: Create application directories
      file:
        path: "{{ item }}"
        state: directory
        owner: "{{ app_user }}"
        group: "{{ app_user }}"
        mode: '0755'
      loop:
        - "{{ app_dir }}"
        - "{{ app_dir }}/data"
        - "{{ app_dir }}/logs"
        - "{{ app_dir }}/config"
        - "{{ app_dir }}/backups"
    
    - name: Copy Docker Compose file
      template:
        src: docker-compose.yml.j2
        dest: "{{ app_dir }}/docker-compose.yml"
        owner: "{{ app_user }}"
        group: "{{ app_user }}"
        mode: '0644'
      notify: restart application
    
    - name: Copy environment file
      template:
        src: .env.j2
        dest: "{{ app_dir }}/.env"
        owner: "{{ app_user }}"
        group: "{{ app_user }}"
        mode: '0600'
      notify: restart application
    
    - name: Copy configuration files
      copy:
        src: "{{ item.src }}"
        dest: "{{ app_dir }}/config/{{ item.dest }}"
        owner: "{{ app_user }}"
        group: "{{ app_user }}"
        mode: '0644'
      loop:
        - { src: "config/production.json", dest: "production.json" }
        - { src: "config/scraping-targets.json", dest: "scraping-targets.json" }
      notify: restart application
    
    - name: Create systemd service
      template:
        src: scraper.service.j2
        dest: /etc/systemd/system/{{ app_name }}.service
        mode: '0644'
      notify:
        - reload systemd
        - restart application
    
    - name: Pull Docker images
      docker_compose:
        project_src: "{{ app_dir }}"
        pull: yes
      become_user: "{{ app_user }}"
    
    - name: Start and enable service
      systemd:
        name: "{{ app_name }}"
        enabled: yes
        state: started
        daemon_reload: yes
    
    - name: Setup log rotation
      copy:
        dest: /etc/logrotate.d/{{ app_name }}
        content: |
          {{ app_dir }}/logs/*.log {
            daily
            missingok
            rotate 7
            compress
            notifempty
            create 644 {{ app_user }} {{ app_user }}
            postrotate
              systemctl reload {{ app_name }}
            endscript
          }
    
    - name: Setup backup cron job
      cron:
        name: "{{ app_name }} backup"
        minute: "0"
        hour: "2"
        user: "{{ app_user }}"
        job: "{{ app_dir }}/scripts/backup.sh"
        cron_file: "{{ app_name }}_backup"
    
    - name: Setup monitoring
      copy:
        src: monitoring/node_exporter.service
        dest: /etc/systemd/system/node_exporter.service
        mode: '0644'
      notify:
        - reload systemd
        - start node exporter

  handlers:
    - name: reload systemd
      systemd:
        daemon_reload: yes
    
    - name: restart application
      systemd:
        name: "{{ app_name }}"
        state: restarted
    
    - name: start node exporter
      systemd:
        name: node_exporter
        enabled: yes
        state: started
```

---

## 🎓 Kurs Tamamlama ve İleri Adımlar

### Öğrenilen Konuların Özeti

Bu kapsamlı kurs boyunca şunları öğrendiniz:

1. **Temel Puppeteer API** kullanımı
2. **Dinamik içerik** ve SPA scraping
3. **Anti-bot korumaları** aşma teknikleri
4. **Performans optimizasyonu** ve ölçeklenebilirlik
5. **Etik scraping** uygulamaları
6. **Test otomasyonu** implementasyonu
7. **Hata yönetimi** ve güvenilir kod yazma

### Proje Örnekleri

Kursumuzda ele alınan başlıca proje türleri:

- **E-ticaret fiyat takibi**
- **Sosyal medya analizi**
- **İçerik toplama sistemleri**
- **Otomatik test senaryoları**
- **Performance monitoring**

### İleri Düzey Konular

Gelişiminizi sürdürmek için önerilen konular:

- **Kubernetes** ile containerization
- **Docker** deployment
- **Machine Learning** ile veri analizi
- **GraphQL** API integration
- **WebSocket** real-time scraping

### Kaynaklar ve Araçlar

- [Puppeteer Resmi Dokümantasyonu](https://pptr.dev)
- [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/)
- [Playwright](https://playwright.dev) - Alternatif framework
- [Selenium](https://selenium.dev) - Cross-browser testing
- [Scrapy](https://scrapy.org) - Python scraping framework

---

## 📞 Destek ve İletişim

Bu kurs materyalinde eksik veya hatalı gördüğünüz kısımlar için katkıda bulunabilirsiniz. Web scraping ve bot geliştirme alanında sürekli gelişen teknolojileri takip etmeyi unutmayın.

**Başarılarınızda!** 🚀

---

*Bu doküman sürekli güncellenmektedir. En son sürüm için repositorimizi takip edin.*