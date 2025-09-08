# Puppeteer ile Web Scraping

**Baştan sona, pratik odaklı, kopyala-yapıştır uyumlu rehber**

---

## 0) Kurulum ve hızlı başlangıç

```bash
# Proje klasörünüzde
npm init -y
npm i puppeteer
# (İsteğe bağlı yardımcılar)
npm i p-queue lodash
```

### En basit çalışma iskeleti

```js
// scrape.js  (Node 18+ / ESM)
import puppeteer from 'puppeteer';

async function main() {
  const browser = await puppeteer.launch({
    headless: true,              // görünmez mod (varsayılan: true)
    args: ['--no-sandbox'],      // CI/Container içinde faydalı
    defaultViewport: { width: 1366, height: 850 },
  });

  const page = await browser.newPage();
  await page.goto('https://example.com', { waitUntil: 'domcontentloaded', timeout: 60_000 }); // :contentReference[oaicite:0]{index=0}
  const title = await page.title();
  console.log({ title });

  await browser.close();
}

main().catch(err => {
  console.error(err);
  process.exit(1);
});
```

> `page.goto(url, { waitUntil })` ile yükleme stratejisini belirleyin:
> `load`, `domcontentloaded`, `networkidle0`, `networkidle2`. Scraping’de çoğu zaman `domcontentloaded` yeterli; çok AJAX’lı sitelerde `networkidle2` tercih edilir. ([pptr.dev][1], [Webshare][2])

---

## 1) Seçiciler (Selectors) ve Locator API

Puppeteer; **CSS**, **ARIA**, **metne göre `::-p-text`**, **XPath**, **pierce (shadow DOM için)** gibi seçicileri destekler. **Locator API** ile otomatik bekleme/yeniden deneme elde edersiniz.

```js
// CSS
await page.waitForSelector('article h2');                        // :contentReference[oaicite:2]{index=2}
const items = await page.$$eval('article h2', els => els.map(e => e.textContent.trim()));

// ARIA (erişilebilirlik adına göre)
await page.locator('::-p-aria(Submit)').click();                 // :contentReference[oaicite:3]{index=3}

// Metne göre
await page.locator('::-p-text(Devam)').click();                  // :contentReference[oaicite:4]{index=4}

// XPath (gerekliyse)
const [btn] = await page.$x("//button[contains(., 'Kaydet')]");
if (btn) await btn.click();

// Shadow DOM (pierce)
const price = await page.$eval('pierce/.product-price', el => el.textContent.trim()); // :contentReference[oaicite:5]{index=5}
```

> **İpucu:** ARIA ve `::-p-text` değişken DOM yapılarında CSS’ten daha sağlam olur. ([Chrome for Developers][3])

---

## 2) Sayfaya gitme (navigasyon) ve bekleme kalıpları

### Doğrudan URL’e gitmek

```js
await page.goto(url, { waitUntil: 'networkidle2', timeout: 60_000 }); // :contentReference[oaicite:7]{index=7}
```

### Bir linke tıklayıp navigasyonu beklemek

```js
await Promise.all([
  page.waitForNavigation({ waitUntil: 'domcontentloaded' }),    // tıklama navigasyonu tetikleyecek
  page.click('a.next-page'),
]);
```

> **SPA’lerde** bazen tam navigasyon olmaz. O durumda URL değişimini veya bir belirtiyi bekleyin:

```js
const oldUrl = page.url();
await page.click('a.to-details');
// URL değişene kadar bekle
await page.waitForFunction(old => location.href !== old, {}, oldUrl);
// ya da içerik belirtisi
await page.waitForSelector('#details-view');
```

### Yeni sekme/popup açılışını yakalamak

```js
const [popup] = await Promise.all([
  new Promise(resolve => page.browserContext().once('targetcreated', async target => {
    const p = await target.page(); if (p) resolve(p);
  })),
  page.click('a[target=_blank]'),
]);
// popup artık bir Page örneği
await popup.bringToFront();
await popup.waitForSelector('h1');
```

---

## 3) Kaydırma (scroll) — sonsuz akış & belirli bir kapsayıcıyı kaydırma

### Tüm sayfayı en alta kadar kademeli kaydırmak

```js
async function autoScroll(page, step = 600, delay = 80, stallLimit = 3) {
  let lastHeight = await page.evaluate('document.body.scrollHeight');
  let stalled = 0;

  while (true) {
    await page.evaluate(y => window.scrollBy(0, y), step);
    await page.waitForTimeout(delay);

    const newHeight = await page.evaluate('document.body.scrollHeight');
    if (newHeight === lastHeight) {
      stalled++;
      if (stalled >= stallLimit) break;  // yeni içerik gelmiyor
    } else {
      stalled = 0;
      lastHeight = newHeight;
    }
  }
}

await autoScroll(page);
```

### Belirli bir kapsayıcıyı aşağı kaydırmak (ör. `.list` içinde sonsuz akış)

```js
async function scrollContainer(page, selector, step = 500, delay = 80) {
  await page.waitForSelector(selector);
  await page.evaluate(async ({ selector, step, delay }) => {
    const el = document.querySelector(selector);
    const sleep = ms => new Promise(r => setTimeout(r, ms));
    let last = 0, same = 0;

    while (true) {
      el.scrollBy(0, step);
      await sleep(delay);
      const sh = el.scrollHeight;
      if (sh === last) {
        same++;
        if (same > 3) break;
      } else {
        same = 0;
        last = sh;
      }
    }
  }, { selector, step, delay });
}

await scrollContainer(page, '.list');
```

### “Daha Fazla Yükle” düğmesi varsa

```js
while (await page.$('button.load-more')) {
  await Promise.all([
    page.waitForResponse(res => res.url().includes('/api/list') && res.ok()),
    page.click('button.load-more'),
  ]);
}
```

---

## 4) Veri çıkarma (extract) kalıpları

### Basit liste

```js
const products = await page.$$eval('.product', cards => cards.map(c => ({
  title: c.querySelector('.title')?.textContent.trim(),
  price: c.querySelector('.price')?.textContent.trim(),
  url: c.querySelector('a.view')?.href,
})));
```

### Tüm linkleri (tam URL’le) toplamak ve tekillemek

```js
const hrefs = await page.$$eval('a', as => as
  .map(a => new URL(a.getAttribute('href') || '', location.href).href)
  .filter(Boolean));

const unique = [...new Set(hrefs)];
```

### Ayrıntı sayfasından alanlar

```js
await page.waitForSelector('h1.product-title');
const data = await page.evaluate(() => ({
  title: document.querySelector('h1.product-title')?.textContent.trim(),
  sku: document.querySelector('dt:contains("SKU") + dd')?.textContent.trim(),
  specs: [...document.querySelectorAll('.specs li')].map(li => li.textContent.trim()),
}));
```

---

## 5) Performans ve stabilite

### Gereksiz istekleri engelle (resim, font, stylesheet)

```js
await page.setRequestInterception(true); // :contentReference[oaicite:8]{index=8}
page.on('request', req => {
  const blocked = ['image', 'stylesheet', 'font', 'media'];
  if (blocked.includes(req.resourceType())) req.abort();
  else req.continue();
});
```

### Zaman aşımı ve otomatik tekrar

```js
async function withRetry(fn, { retries = 3, baseDelay = 500 } = {}) {
  let attempt = 0;
  while (true) {
    try { return await fn(); }
    catch (err) {
      if (++attempt > retries) throw err;
      await new Promise(r => setTimeout(r, baseDelay * 2 ** (attempt - 1)));
    }
  }
}
```

### Paralel çalıştırma (nazikçe)

```js
import PQueue from 'p-queue';

const queue = new PQueue({ concurrency: 3, interval: 1000, intervalCap: 6 }); // 6 istek/sn
const urls = [...]; // hedef URL listesi

for (const u of urls) {
  queue.add(async () => {
    const page = await browser.newPage();
    try {
      await page.goto(u, { waitUntil: 'domcontentloaded' });
      // ... veri çek
    } finally {
      await page.close();
    }
  });
}

await queue.onIdle();
```

### Tek tarayıcı, çok sayfa / çok context

* **Aynı oturum/cookie** gerekmiyorsa `browser.createBrowserContext()` ile izole context kullanın.
* **Sayfa sayısını** makine gücünüze göre sınırlayın (CPU/RAM).

---

## 6) Sayfalama (pagination) kalıpları

#### “Sonraki” düğmesi ile

```js
while (true) {
  // mevcut sayfayı topla
  const rows = await page.$$eval('.row', els => els.map(e => e.textContent.trim()));
  // ileri git
  const next = await page.$('a.next:not(.disabled)');
  if (!next) break;
  await Promise.all([ page.waitForNavigation(), next.click() ]);
}
```

#### Sayfa numarası ile

```js
for (let p = 1; p <= 50; p++) {
  const url = `https://site.com/list?page=${p}`;
  await page.goto(url, { waitUntil: 'domcontentloaded' });
  // ... çek
}
```

---

## 7) Formlar, klavye, mouse

```js
await page.type('#q', 'laptop', { delay: 50 });   // yazarken hafif gecikme
await page.keyboard.press('Enter');
await page.waitForNavigation();
await page.click('label[for="filter-new"]');
await page.mouse.move(200, 300);
await page.mouse.wheel({ deltaY: 800 });          // scroll ile kaydır
```

> Etkileşim ayrıntıları ve kısa yollar için rehbere bakın. ([pptr.dev][4])

---

## 8) İstek/yanıt dinleme (Network), XHR/Fetch’ten veri çekme

```js
// Belirli API yanıtını bekleyip JSON almak
const [resp] = await Promise.all([
  page.waitForResponse(r => r.url().includes('/search') && r.request().method() === 'POST' && r.ok()),
  page.click('button.search'),
]);
const json = await resp.json();
```

```js
// Tüm istekleri gözetlemek
page.on('request', req => console.log('REQ', req.method(), req.url()));
page.on('response', res => console.log('RES', res.status(), res.url()));
```

---

## 9) Kimlik doğrulama, çerezler, proxy

### Temel kimlik doğrulama (Basic)

```js
await page.authenticate({ username: 'user', password: 'pass' }); // :contentReference[oaicite:10]{index=10}
```

### Çerez okuma/yazma

```js
const cookies = await page.cookies();
await page.setCookie({ name: 'token', value: 'abc', domain: 'site.com' });
```

### Proxy kullanmak

```js
const browser = await puppeteer.launch({
  headless: true,
  args: ['--proxy-server=http://host:port'],
});
// Varsa kullanıcı/parola
const page = await browser.newPage();
await page.authenticate({ username: 'user', password: 'pass' });
```

---

## 10) Dosya indirme / yükleme

* **Puppeteer şu anda indirmeleri birinci sınıf API ile yönetmez.** Alternatifler:

  1. İndirme bağlantısını alıp Node tarafında `fetch/axios` ile indirmek.
  2. CDP (`page.target().createCDPSession()`) ile indirme klasörü ayarlamak (daha ileri seviye, Chrome’a özgü).
* **Yükleme** için dosya input’una `uploadFile` çağrın.

```js
// Upload
const input = await page.waitForSelector('input[type=file]');
await input.uploadFile('./files/invoice.pdf'); // :contentReference[oaicite:11]{index=11}
```

> İndirme konusunda “Files” kılavuzundaki sınırlamayı ve yaklaşımları dikkate alın. ([pptr.dev][5])

---

## 11) PDF, ekran görüntüsü, HTML içerik

```js
await page.screenshot({ path: 'page.png', fullPage: true });
await page.pdf({ path: 'page.pdf', format: 'A4', printBackground: true });
const html = await page.content(); // render edilmiş HTML
```

---

## 12) Hatalar, zaman aşımı ve yeniden başlatma

```js
page.setDefaultTimeout(30_000);
page.setDefaultNavigationTimeout(60_000);

page.on('error', e => console.error('Page error:', e));
page.on('pageerror', e => console.error('Console error in page:', e)); // script hataları

// Navigasyonun asılı kalmasını önlemek için:
await Promise.race([
  page.waitForNavigation({ waitUntil: 'networkidle2' }),
  page.waitForTimeout(25_000),
]);
```

---

## 13) Etik, hukuki ve operasyonel notlar

* **robots.txt** ve **kullanım şartları**na uyun; yoğun trafiği engellemek için hız sınırlaması uygulayın.
* Hedef sisteme zarar vermemek için **concurrency** ve **bekleme** ayarlarını muhafazakâr tutun.
* Kişisel veriler işleniyorsa **KVKK/GDPR** gerekliliklerini gözetin.

---

## 14) Örnek: Liste → Detay çok adımlı scraping

```js
import puppeteer from 'puppeteer';

async function scrapeListPage(page, url) {
  await page.goto(url, { waitUntil: 'domcontentloaded' });
  await autoScroll(page);

  // Listeyi topla
  const items = await page.$$eval('.card', cards => cards.map(c => ({
    title: c.querySelector('.title')?.textContent.trim(),
    href: new URL(c.querySelector('a.details')?.getAttribute('href') || '', location.href).href,
  })));

  return items;
}

async function scrapeDetailPage(page, url) {
  await page.goto(url, { waitUntil: 'domcontentloaded' });
  await page.waitForSelector('h1.product-title');

  return await page.evaluate(() => ({
    title: document.querySelector('h1.product-title')?.textContent.trim(),
    price: document.querySelector('.price')?.textContent.trim(),
    desc: document.querySelector('.desc')?.textContent.trim(),
  }));
}

async function autoScroll(page, step = 600) {
  let last = 0, same = 0;
  while (true) {
    await page.evaluate(y => scrollBy(0, y), step);
    await page.waitForTimeout(80);
    const h = await page.evaluate('document.body.scrollHeight');
    if (h === last) { if (++same > 3) break; } else { same = 0; last = h; }
  }
}

async function main() {
  const browser = await puppeteer.launch({ headless: true });
  const page = await browser.newPage();

  // Gereksizleri kapat
  await page.setRequestInterception(true);
  page.on('request', req => ['image','stylesheet','font','media'].includes(req.resourceType()) ? req.abort() : req.continue());

  const listUrl = 'https://site.com/products';
  const list = await scrapeListPage(page, listUrl);

  const results = [];
  for (const { href } of list.slice(0, 50)) {
    const detail = await scrapeDetailPage(page, href);
    results.push({ url: href, ...detail });
  }

  console.log(JSON.stringify(results, null, 2));
  await browser.close();
}

main();
```

---

## 15) Sık kullanılan API’ler (mini başvuru)

* `puppeteer.launch({ headless: true | false | 'shell' })` — `'shell'` eski “headless-shell” ikilisini kullanır. ([pptr.dev][6])
* `browser.newPage()`, `browser.createBrowserContext()`
* `page.goto(url, { waitUntil, timeout })` — navigasyon. ([pptr.dev][1])
* `page.waitForSelector(sel)` — elemanı bekler. ([pptr.dev][7])
* `page.$(sel) / page.$$(sel) / page.$eval / page.$$eval` — seç ve değerlendir. ([pptr.dev][8])
* `page.locator(selector)` + `::-p-aria`, `::-p-text` — güvenilir seçim/aksiyon. ([pptr.dev][4], [Stack Overflow][9])
* `page.click / type / keyboard.press / mouse.wheel` — etkileşimler. ([pptr.dev][4])
* `page.setRequestInterception(true)` + `page.on('request')` — istekleri filtrele. ([Stack Overflow][10])
* `page.authenticate({ username, password })` — Basic auth. ([GitHub][11])
* `input.uploadFile()` — dosya yükleme. ([pptr.dev][5])
* `page.screenshot / page.pdf / page.content` — çıktı almak.
* WebDriver **BiDi** desteği hakkında: bkz. rehber. ([pptr.dev][12])

---

## 16) Sorun giderme / yapılandırma

* **Kurulum boyutu / tarayıcı indirme konumu**: Puppeteer, Chrome for Testing ve `chrome-headless-shell` indirir; varsayılan cache: `~/.cache/puppeteer`. Ortam değişkenleri ve yapılandırmalar için bakınız **Installation/Configuration**. ([pptr.dev][13])
* **Yapılandırma dosyası**: `.puppeteerrc.*` veya `puppeteer.config.*`. ([pptr.dev][14])

---

## 17) İleri konular için resmi kılavuzlar

* **Getting started** (başlangıç): temel akışlar ve örnekler. ([pptr.dev][15])
* **Page interactions**: tıklama, yazma, seçiciler, ARIA, p-selectors. ([pptr.dev][4])
* **waitForSelector** ayrıntıları. ([pptr.dev][7])
* **goto** ve bekleme stratejileri. ([pptr.dev][1])
* **Request interception**: istekleri yakalama/iptal/continue. ([Stack Overflow][10])
* **Files** (indirme/yükleme notları). ([pptr.dev][5])
* **WebDriver BiDi** desteği. ([pptr.dev][12])

---

Bu belge, scraping için **linke gitme**, **element aşağı kaydırma**, **liste → detay** akışları, **bekleme stratejileri**, **performans/istikrar** ve **ağ dinleme** dahil olmak üzere gerekli tüm pratik kalıpları örnek kodlarla kapsamaktadır. Kopyala-yapıştırla doğrudan kullanabilirsiniz; gerekirse modüllerinizi ve hedef seçicilerinizi projeye göre uyarlayın.

[1]: https://pptr.dev/api/puppeteer.page.goto?utm_source=chatgpt.com "Page.goto() method - Puppeteer"
[2]: https://www.webshare.io/academy-article/puppeteer-wait-for-page-to-load?utm_source=chatgpt.com "Wait For Page to Load in Puppeteer: 4 Methods Compared"
[3]: https://developer.chrome.com/blog/puppetaria?utm_source=chatgpt.com "Puppetaria: accessibility-first Puppeteer scripts | Blog"
[4]: https://pptr.dev/guides/page-interactions?utm_source=chatgpt.com "Page interactions - Puppeteer"
[5]: https://pptr.dev/guides/files?utm_source=chatgpt.com "Files - Puppeteer"
[6]: https://pptr.dev/api/puppeteer.launchoptions?utm_source=chatgpt.com "LaunchOptions interface - Puppeteer"
[7]: https://pptr.dev/api/puppeteer.page.waitforselector?utm_source=chatgpt.com "Page.waitForSelector() method - Puppeteer"
[8]: https://pptr.dev/api/puppeteer.page._?utm_source=chatgpt.com "Page.$() method - Puppeteer"
[9]: https://stackoverflow.com/questions/47407791/how-do-you-click-on-an-element-with-text-in-puppeteer?utm_source=chatgpt.com "How do you click on an element with text in Puppeteer?"
[10]: https://stackoverflow.com/questions/63818869/why-does-headless-need-to-be-false-for-puppeteer-to-work?utm_source=chatgpt.com "Why does headless need to be false for Puppeteer to work?"
[11]: https://github.com/puppeteer/puppeteer/issues/3711?utm_source=chatgpt.com "puppeteer 1.11.0 ignore --proxy-server for localhost URLs #3711"
[12]: https://pptr.dev/webdriver-bidi?utm_source=chatgpt.com "WebDriver BiDi support - Puppeteer"
[13]: https://pptr.dev/guides/installation?utm_source=chatgpt.com "Installation - Puppeteer"
[14]: https://pptr.dev/guides/configuration?utm_source=chatgpt.com "Configuration - Puppeteer"
[15]: https://pptr.dev/guides/getting-started?utm_source=chatgpt.com "Getting started - Puppeteer"
