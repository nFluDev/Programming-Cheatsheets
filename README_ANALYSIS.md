# React + Redux Sepet Uygulaması – Kapsamlı Dokümantasyon

## 1. Temel Kavramlar

### React Nedir?
React, kullanıcı arayüzleri oluşturmak için tasarlanmış bileşen tabanlı bir JavaScript kütüphanesidir. Uygulamalar, tekrar kullanılabilir bileşenlerin (components) hiyerarşik olarak bir araya getirilmesiyle inşa edilir. React'in Virtual DOM yaklaşımı, gerçek DOM güncellemelerini minimize ederek performans kazancı sağlar. Eyalet (state) değişiklikleri olduğunda React, UI'ın hangi parçalarının güncelleneceğini verimli şekilde belirler ve sadece gerekli bileşenleri yeniden render eder. Bu projede React, sayfa bileşenlerinin (kategori listesi, ürün listesi, sepet özetleri vb.) hem yapısını hem de kullanıcı etkileşimlerini yönetmek için kullanılır.

### Redux Nedir?
Redux, JavaScript uygulamaları için öngörülebilir bir state yönetim kütüphanesidir. Uygulamanın global durumunu tek bir store içerisinde saklar. Veri akışı tek yönlüdür: bileşenler `dispatch` ile action gönderir, reducer'lar bu action'ları yorumlayarak yeni state'i üretir ve store bu güncel state'i bileşenlere iletir. Temel parçalar şöyle özetlenebilir:
- **Store:** Uygulamanın tekil veri kaynağıdır. Bütün state burada tutulur.
- **Actions:** State'i değiştirme niyetini ifade eden düz nesnelerdir. Her action, `type` alanı sayesinde reducer'lara hangi işlemin yapılması gerektiğini söyler.
- **Reducers:** Action'ları dinleyerek state'in nasıl güncelleneceğine karar veren saf fonksiyonlardır.
- **Dispatch:** Action'ı store'a gönderme mekanizmasıdır. Bileşenler, kullanıcı etkileşimleri sonrası `dispatch` çağırarak state güncellemelerini tetikler.
Bu projede Redux, kategoriler, ürünler ve alışveriş sepeti gibi global verilere tüm bileşenlerden tutarlı şekilde erişebilmek için kullanılır. `redux-thunk` ara katmanı sayesinde API çağrıları gibi asenkron işlemler de action olarak yönetilir.

## 2. Proje Yapısı

### 2.1 Klasör Ağacı
```
./
├── .env
├── .gitignore
├── .vscode/
│   └── snippets/
│       ├── javascript.code-snippets
│       └── remote.code-snippets
├── api/
│   └── db.json
├── package-lock.json
├── package.json
├── public/
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
├── README.md
└── src/
    ├── components/
    │   ├── cart/
    │   │   ├── CartDetail.js
    │   │   └── CartSummary.js
    │   ├── categories/
    │   │   └── CategoryList.js
    │   ├── common/
    │   ├── navi/
    │   │   └── Navi.js
    │   ├── products/
    │   │   └── ProductList.js
    │   ├── root/
    │   │   ├── App.js
    │   │   └── Dashboard.js
    │   └── toolbox/
    ├── index.js
    ├── redux/
    │   ├── actions/
    │   │   ├── actionTypes.js
    │   │   ├── cartActions.js
    │   │   ├── categoryActions.js
    │   │   └── productActions.js
    │   └── reducers/
    │       ├── cartReducer.js
    │       ├── categoryListReducer.js
    │       ├── changeCategoryReducer.js
    │       ├── configureStore.js
    │       ├── index.js
    │       ├── initialState.js
    │       └── productListReducer.js
    ├── reportWebVitals.js
    └── setupTests.js
```

### 2.2 Dizin Rollerinin Özeti
- `api/`: JSON Server benzeri yerel servisler için örnek veri kaynağı (`db.json`).
- `public/`: CRA tarafından sunulan statik varlıklar ve `index.html` şablonu.
- `src/components/`: React bileşenleri; alt klasörler işlevsel modüllere ayrılmıştır (sepet, ürünler, navigasyon vb.).
- `src/redux/`: Uygulama durumunu yöneten action ve reducer tanımları ile store kurulumunu içerir.
- `src/index.js`: Uygulamanın giriş noktası; React DOM render'ı, router ve Redux provider kurulumları burada.
- `src/reportWebVitals.js` ve `src/setupTests.js`: CRA'nın performans ve test altyapısı için yardımcı dosyaları.
- `.env`: Geliştirme sunucusunun portunu özelleştirir.
- `package.json`: Proje bağımlılıkları, script'ler ve CRA yapılandırmaları.

## 3. Dosya Bazlı Analiz

Aşağıdaki bölümlerde her kaynak dosyanın tam içeriği ve satır satır açıklamaları yer alır. Satır numaraları, kod bloğundaki biçim üzerinden referanslanmıştır.

### `Dosya: .env`

**Genel Görev:** CRA geliştirme sunucusunun varsayılan portunu `3000` yerine `5001` olarak değiştirmek için kullanılan ortam değişkenini sağlar.

**Kod Analizi:**
```ini
PORT=5001
```

**Satır Satır Açıklama:**
- `PORT=5001`: Create React App sunucusunun dinleyeceği portu 5001 olarak belirler; böylece varsayılan port başka bir süreç tarafından kullanılıyorsa çakışma engellenir.

### `Dosya: package.json`

**Genel Görev:** Projenin metadata bilgilerini, bağımlılıklarını ve npm script'lerini tanımlar; CRA tarafından kullanılan derleme ayarlarını içerir.

**Kod Analizi:**
```json
{
  "name": "test",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/dom": "^10.4.1",
    "@testing-library/jest-dom": "^6.9.1",
    "@testing-library/react": "^16.3.0",
    "@testing-library/user-event": "^13.5.0",
    "alertifyjs": "^1.14.0",
    "bootstrap": "^5.3.8",
    "react": "^19.2.0",
    "react-dom": "^19.2.0",
    "react-redux": "^9.2.0",
    "react-router-dom": "^7.9.5",
    "react-scripts": "5.0.1",
    "reactstrap": "^9.2.3",
    "redux": "^5.0.1",
    "redux-thunk": "^3.1.0",
    "web-vitals": "^2.1.4"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
```

**Satır Satır Açıklama:**
- `1`: JSON objesinin açılışı.
- `2`: `name` alanı paket adını `test` olarak tanımlar.
- `3`: `version` projenin şu anki sürümünü belirtir.
- `4`: `private: true` değeri paketin npm'e yayımlanmasını engeller.
- `5`: `dependencies` anahtarının açılışı; uygulamanın çalışma zamanında ihtiyaç duyduğu paketleri listeler.
- `6-9`: Testing Library paketleri, bileşen testleri için gerekli yardımcı araçları ekler.
- `10`: `alertifyjs` bildirim kütüphanesini bağımlılık olarak ekler.
- `11`: `bootstrap` CSS framework'ünü dahil eder.
- `12-13`: `react` ve `react-dom` sürümlerini verir; çekirdek UI ve DOM bağlayıcısı.
- `14`: `react-redux` ile Redux store'unu React bileşenlerine bağlamak mümkündür.
- `15`: `react-router-dom` istemci tarafı yönlendirme desteği sağlar.
- `16`: `react-scripts` CRA CLI görevlerini sağlar.
- `17`: `reactstrap` Bootstrap bileşenlerinin React sarmalayıcısıdır.
- `18`: `redux` ana state yönetimi kütüphanesidir.
- `19`: `redux-thunk` asenkron action yaratıcılarını destekleyen ara katmandır.
- `20`: `web-vitals` performans ölçümleri için kullanılabilir.
- `21`: `dependencies` objesinin kapanışıdır.
- `22`: `scripts` bölümünü başlatır; npm komut kısayolları.
- `23`: `start` script'i `react-scripts start` ile geliştirme sunucusunu çalıştırır.
- `24`: `build` script'i prodüksiyon build'i üretir.
- `25`: `test` script'i Jest tabanlı testleri izleme modunda çalıştırır.
- `26`: `eject` script'i CRA yapılandırmalarını projeye kopyalar.
- `27`: `scripts` objesinin kapanışıdır.
- `28`: `eslintConfig` alanı ESLint uzantılarını belirtir.
- `29-32`: `react-app` ve `react-app/jest` ayarlarının devralındığını gösterir.
- `33`: `eslintConfig` blok kapanışı.
- `34`: `browserslist` bölümünü başlatır; hedef tarayıcıları tanımlar.
- `35-39`: Üretim için desteklenen tarayıcılar.
- `40-44`: Geliştirme sırasında desteklenen tarayıcılar.
- `45`: `browserslist` objesinin sonu.
- `46`: Ana JSON objesinin kapanışı.
### `Dosya: public/index.html`

**Genel Görev:** Create React App tarafından derlenen uygulamanın en temel HTML şablonunu sağlar; React komponentleri uygulama çalıştığında `#root` elemanına bağlanır.

**Kod Analizi:**
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <!--
      manifest.json provides metadata used when your web app is installed on a
      user's mobile device or desktop. See https://developers.google.com/web/fundamentals/web-app-manifest/
    -->
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <!--
      Notice the use of %PUBLIC_URL% in the tags above.
      It will be replaced with the URL of the `public` folder during the build.
      Only files inside the `public` folder can be referenced from the HTML.

      Unlike "/favicon.ico" or "favicon.ico", "%PUBLIC_URL%/favicon.ico" will
      work correctly both with client-side routing and a non-root public URL.
      Learn how to configure a non-root public URL by running `npm run build`.
    -->
    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
    <!--
      This HTML file is a template.
      If you open it directly in the browser, you will see an empty page.

      You can add webfonts, meta tags, or analytics to this file.
      The build step will place the bundled scripts into the <body> tag.

      To begin the development, run `npm start` or `yarn start`.
      To create a production bundle, use `npm run build` or `yarn build`.
    -->
  </body>
</html>
```

**Satır Satır Açıklama:**
- `1`: HTML5 belgesini tanımlar.
- `2`: Sayfa dilinin İngilizce olduğunu belirtir.
- `3-4`: `<head>` etiketi ve açılışı.
- `5`: UTF-8 karakter seti desteği.
- `6`: Favikon dosyasına bağlanır; `%PUBLIC_URL%` build sırasında gerçek public yoluna çevrilir.
- `7`: Responsive tasarım için viewport meta ayarı.
- `8`: Mobil tarayıcılarda adres çubuğu rengini belirler.
- `9-11`: Sayfanın açıklamasını içeren meta tag.
- `12`: iOS cihazlar için ikon tanımı.
- `13-18`: Manifest dosyasının kullanımını açıklayan yorum bloğu.
- `19`: Uygulamanın manifest dosyasına bağlantı ekler.
- `20-27`: Build sırasında `%PUBLIC_URL%` kullanımını anlatan ikinci yorum bloğu.
- `28`: Sekme başlığını belirleyen `<title>` etiketi.
- `29`: `<head>` kapanışı.
- `30`: `<body>` açılışı.
- `31`: JavaScript kapalıysa kullanıcıyı bilgilendiren `<noscript>` elementi.
- `32`: React uygulamasının render edileceği `root` div'i.
- `33-40`: CRA tarafından eklenen bilgilendirici yorumlar; template kullanımını açıklar.
- `41`: `<body>` kapanışı.
- `42`: `</html>` kapanışı.

### `Dosya: public/manifest.json`

**Genel Görev:** Progressive Web App (PWA) desteği için uygulamanın adını, ikonlarını ve tema ayarlarını tanımlar.

**Kod Analizi:**
```json
{
  "short_name": "React App",
  "name": "Create React App Sample",
  "icons": [
    {
      "src": "favicon.ico",
      "sizes": "64x64 32x32 24x24 16x16",
      "type": "image/x-icon"
    },
    {
      "src": "logo192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "logo512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": ".",
  "display": "standalone",
  "theme_color": "#000000",
  "background_color": "#ffffff"
}
```

**Satır Satır Açıklama:**
- `1`: JSON objesinin başlangıcı.
- `2`: Kısa ad; cihaz ana ekranlarında gösterilir.
- `3`: Uygulamanın tam adı.
- `4`: `icons` dizisini açar; PWA ikonları.
- `5-8`: Favicon boyutları ve MIME tipini tanımlar.
- `9-12`: 192x192 boyutlu PNG ikon bilgisi.
- `13-16`: 512x512 boyutlu PNG ikon bilgisi.
- `17`: `icons` dizisinin kapanışı.
- `18`: `start_url` uygulamanın başlangıç yolunu belirtir.
- `19`: `display` modunu `standalone` yaparak uygulama benzeri davranışı etkinleştirir.
- `20`: Tema rengini siyah olarak ayarlar.
- `21`: Arka plan rengini beyaz yapar.
- `22`: JSON objesini kapatır.

### `Dosya: public/robots.txt`

**Genel Görev:** Arama motoru tarayıcılarına hangi sayfalara erişebileceklerini bildiren kuralları içerir; varsayılan olarak tüm botlara izin verir.

**Kod Analizi:**
```txt
# https://www.robotstxt.org/robotstxt.html
User-agent: *
Disallow:
```

**Satır Satır Açıklama:**
- `1`: robots.txt formatı hakkında referans veren yorum satırı.
- `2`: Kuralların tüm user-agent'lar (botlar) için geçerli olduğunu belirtir.
- `3`: `Disallow` için boş değer vererek hiçbir yolun engellenmediğini, yani sitenin tamamının taranabilir olduğunu ifade eder.

### `Dosya: api/db.json`

**Genel Görev:** Yerel JSON API veya JSON Server için kullanılabilecek örnek ürün ve kategori verilerini barındırır; Redux action'larının beklediği veri şemasını gösterir.

**Kod Analizi:**
```json
{
    "products": [
        {
            "id": 1,
            "categoryId": 1,
            "name": "Product A",
            "price": 29.99,
            "inStock": true
        },
        {
            "id": 2,
            "categoryId": 2,
            "name": "Product B",
            "price": 49.99,
            "inStock": false
        },
        {
            "id": 3,
            "categoryId": 1,
            "name": "Product C",
            "price": 19.99,
            "inStock": true
        },
        {
            "id": 4,
            "categoryId": 3,
            "name": "Product D",
            "price": 99.99,
            "inStock": true
        },
        {
            "id": 5,
            "categoryId": 2,
            "name": "Product E",
            "price": 39.99,
            "inStock": false
        },
        {
            "id": 6,
            "categoryId": 3,
            "name": "Product F",
            "price": 59.99,
            "inStock": true
        },
        {
            "id": 7,
            "categoryId": 1,
            "name": "Product G",
            "price": 24.99,
            "inStock": true
        },
        {
            "id": 8,
            "categoryId": 2,
            "name": "Product H",
            "price": 44.99,
            "inStock": false
        }
        ,{
            "id": 9,
            "categoryId": 3,
            "name": "Product I",
            "price": 79.99,
            "inStock": true
        },
        {
            "id": 10,
            "categoryId": 1,
            "name": "Product J",
            "price": 14.99,
            "inStock": true
        },
        {
            "id": 11,
            "categoryId": 2,
            "name": "Product K",
            "price": 54.99,
            "inStock": false
        },
        {
            "id": 12,
            "categoryId": 3,
            "name": "Product L",
            "price": 89.99,
            "inStock": true
        },
        {
            "id": 13,
            "categoryId": 1,
            "name": "Product M",
            "price": 34.99,
            "inStock": true
        },
        {
            "id": 14,
            "categoryId": 2,
            "name": "Product N",
            "price": 64.99,
            "inStock": false
        },
        {
            "id": 15,
            "categoryId": 3,
            "name": "Product O",
            "price": 109.99,
            "inStock": true
        },
        {
            "id": 16,
            "categoryId": 1,
            "name": "Product P",
            "price": 22.99,
            "inStock": true
        },
        {
            "id": 17,
            "categoryId": 2,
            "name": "Product Q",
            "price": 48.99,
            "inStock": false
        },
        {
            "id": 18,
            "categoryId": 3,
            "name": "Product R",
            "price": 92.99,
            "inStock": true
        }
    ],
    "categories": [
        {
            "id": 1,
            "name": "Category 1"
        },
        {
            "id": 2,
            "name": "Category 2"
        },
        {
            "id": 3,
            "name": "Category 3"
        }
    ]
}
```

**Satır Satır Açıklama:**
- `1`: JSON kök objeyi açar.
- `2`: `products` dizisinin başladığını gösterir.
- `3-10`: ID'si 1 olan, kategori 1'e bağlı, stokta bulunan `Product A` öğesinin alanlarını tanımlar.
- `11-18`: ID 2 olan ve stoksuz `Product B` tanımı.
- `19-26`: `Product C` verisi; kategori 1, stokta.
- `27-34`: `Product D`; kategori 3 ve stokta.
- `35-42`: `Product E`; kategori 2 ve stokta değil.
- `43-50`: `Product F`; kategori 3 ve stokta.
- `51-58`: `Product G`; kategori 1 ve stokta.
- `59-66`: `Product H`; kategori 2 ve stokta değil.
- `67-74`: `Product I`; kategori 3 ve stokta.
- `75-82`: `Product J`; kategori 1 ve stokta.
- `83-90`: `Product K`; kategori 2 ve stokta değil.
- `91-98`: `Product L`; kategori 3 ve stokta.
- `99-106`: `Product M`; kategori 1 ve stokta.
- `107-114`: `Product N`; kategori 2 ve stokta değil.
- `115-122`: `Product O`; kategori 3 ve stokta.
- `123-130`: `Product P`; kategori 1 ve stokta.
- `131-138`: `Product Q`; kategori 2 ve stokta değil.
- `139-146`: `Product R`; kategori 3 ve stokta.
- `147`: `products` dizisinin kapanışı.
- `148`: `categories` dizisini açar.
- `149-152`: ID 1 olan kategori tanımı.
- `153-156`: ID 2 olan kategori tanımı.
- `157-160`: ID 3 olan kategori tanımı.
- `161`: `categories` dizisinin kapanışı.
- `162`: JSON kök objenin kapanışı.

### `Dosya: src/index.js`

**Genel Görev:** React uygulamasının giriş noktasıdır; Redux store'unu ve React Router'ı konfigüre ederek `App` bileşenini DOM'a render eder.

**Kod Analizi:**
```javascript
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./components/root/App";
import reportWebVitals from "./reportWebVitals";
import "bootstrap/dist/css/bootstrap.min.css";
import { Provider } from "react-redux";
import { BrowserRouter } from "react-router-dom";
import configureStore from "./redux/reducers/configureStore";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <Provider store={configureStore()}>
        <App />
      </Provider>
    </BrowserRouter>
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

**Satır Satır Açıklama:**
- `1`: React temel kütüphanesini içe aktarır.
- `2`: React 18 ile gelen `createRoot` API'sini kullanabilmek için `react-dom/client` modülünü çağırır.
- `3`: Uygulamanın üst düzey bileşeni `App` import edilir.
- `4`: Performans ölçümleri için kullanılan `reportWebVitals` fonksiyonunu getirir.
- `5`: Bootstrap stil dosyasını global olarak projeye dahil eder.
- `6`: Redux sağlayıcısını (`Provider`) import eder.
- `7`: İstemci tarafı yönlendirme için `BrowserRouter` bileşenini içe aktarır.
- `8`: Redux store'unu oluşturan `configureStore` fonksiyonunu alır.
- `10`: DOM'daki `root` elementini hedefleyen React kökü oluşturulur.
- `11-18`: React bileşen ağacını render eder; StrictMode, Router ve Redux Provider sarmalları kuruludur.
- `12`: `React.StrictMode` geliştirme sırasında ek uyarılar sağlar.
- `13`: `BrowserRouter` yönlendirme yapısını etkinleştirir.
- `14-16`: `Provider`, `configureStore()` tarafından döndürülen store'u alt bileşenlerle paylaşır ve `App` bileşenini içerir.
- `19`: Render işleminin kapanış parantezi.
- `21-23`: CRA tarafından eklenen yorumlar; `reportWebVitals` kullanımını açıklar.
- `24`: `reportWebVitals()` çağrısı; istersek ölçüm fonksiyonunu parametre olarak verebiliriz (şu anda verilmemiştir, dolayısıyla yan etkisi yoktur).

### `Dosya: src/reportWebVitals.js`

**Genel Görev:** Web Vitals metriklerini ölçmek için yardımcı fonksiyon sağlar; CRA'nın performans izleme şablonudur.

**Kod Analizi:**
```javascript
const reportWebVitals = onPerfEntry => {
  if (onPerfEntry && onPerfEntry instanceof Function) {
    import('web-vitals').then(({ getCLS, getFID, getFCP, getLCP, getTTFB }) => {
      getCLS(onPerfEntry);
      getFID(onPerfEntry);
      getFCP(onPerfEntry);
      getLCP(onPerfEntry);
      getTTFB(onPerfEntry);
    });
  }
};

export default reportWebVitals;
```

**Satır Satır Açıklama:**
- `1`: `reportWebVitals` fonksiyonunu tanımlar; opsiyonel `onPerfEntry` callback'i alır.
- `2`: Parametre verildi ve fonksiyon tipindeyse ölçüm yapılacağını kontrol eder.
- `3`: Dinamik import ile `web-vitals` paketinden metrik fonksiyonlarını alır; bundle'a yalnızca gerektiğinde yüklenir.
- `4-8`: Her bir metrik fonksiyonunu `onPerfEntry` callback'i ile çağırır (CLS, FID, FCP, LCP, TTFB).
- `9`: Dinamik import promise'inin kapanışı.
- `10`: `if` bloğunun kapanışı.
- `11`: Fonksiyon bloğunu kapatır.
- `13`: Fonksiyonu varsayılan olarak dışa aktarır.

### `Dosya: src/setupTests.js`

**Genel Görev:** Jest test ortamı başlatıldığında Testing Library'nin jest-dom eşleşmelerini etkinleştirir; CRA'nın varsayılan test ayarıdır.

**Kod Analizi:**
```javascript
// jest-dom adds custom jest matchers for asserting on DOM nodes.
// allows you to do things like:
// expect(element).toHaveTextContent(/react/i)
// learn more: https://github.com/testing-library/jest-dom
import '@testing-library/jest-dom';
```

**Satır Satır Açıklama:**
- `1-4`: Jest-dom'un ne yaptığına dair bilgilendirici yorumlar.
- `5`: `@testing-library/jest-dom` paketini projeye dahil ederek `toBeInTheDocument` gibi matcher'ların testlerde kullanılmasını sağlar.

### `Dosya: src/components/root/App.js`

**Genel Görev:** Uygulamanın üst seviye bileşenidir; navigasyon çubuğunu ve yönlendirme yapılandırmasını içerir.

**Kod Analizi:**
```javascript
import { Route, Routes } from 'react-router-dom';
import Navi from '../navi/Navi';
import Dashboard from './Dashboard';
import { Container } from 'reactstrap';
import CartDetail from '../cart/CartDetail';

function App() {
  return (
    <Container>
      <Navi />
      <Routes>
        <Route path="/" element={<Dashboard />} />
        <Route path="/products" element={<Dashboard />} />
        <Route path="/cart" element={<CartDetail />} />
      </Routes>
    </Container>
  );
}
export default App;
```

**Satır Satır Açıklama:**
- `1`: React Router v6'nın `Routes` ve `Route` bileşenlerini içe aktarır.
- `2`: Üst navigasyon bileşeni `Navi` import edilir.
- `3`: Ana içerik alanını yöneten `Dashboard` bileşeni dahil edilir.
- `4`: Reactstrap'ten `Container` bileşeni, Bootstrap layout'u sağlar.
- `5`: Sepet detay sayfası bileşeni import edilir.
- `7`: Fonksiyonel `App` bileşeni tanımlanır.
- `8-16`: JSX çıktısı; container içinde navigasyon ve yönlendirme yapısı.
- `9`: Bootstrap container'ı oluşturur.
- `10`: Navigasyon çubuğunu render eder.
- `11-15`: Router yapılandırması; her bir route path'e uygun bileşeni render eder.
- `12-13`: Ana sayfa ve `/products` yolu aynı `Dashboard` bileşenini gösterir.
- `14`: `/cart` rotası `CartDetail` bileşenini gösterir.
- `17`: Fonksiyonun kapanış parantezi.
- `18`: `App` bileşenini varsayılan olarak dışa aktarır.

### `Dosya: src/components/root/Dashboard.js`

**Genel Görev:** Ana sayfa düzenini oluşturur; sol tarafta kategoriler, sağ tarafta ürün listesi gösterir.

**Kod Analizi:**
```javascript
import React from "react";
import {Row, Col} from "reactstrap";
import CategoryList from "../categories/CategoryList";
import ProductList from "../products/ProductList";

class Dashboard extends React.Component {
  render() {
    return <div>
      <Row>
        <Col xs="3">
          <CategoryList />
        </Col>
        <Col xs="9">
          <ProductList />
        </Col>
      </Row>
    </div>;
  }
}

export default Dashboard;
```

**Satır Satır Açıklama:**
- `1`: React sınıf bileşeni kullanımını sağlar.
- `2`: Reactstrap'ten grid sistemi için `Row` ve `Col` bileşenleri getirilir.
- `3`: Kategori listesinin bileşeni import edilir.
- `4`: Ürün listesinin bileşeni import edilir.
- `6`: `Dashboard` sınıf bileşeni tanımlanır.
- `7`: `render` metodu UI çıktısını belirler.
- `8-16`: JSX yapısı; kategori ve ürün listelerini iki sütuna böler.
- `9`: 12 kolonluk grid içinde yeni bir satır başlatır.
- `10-12`: Sol sütun genişliğini 3 kolon olarak ayarlar ve `CategoryList` bileşenini render eder.
- `13-15`: Sağ sütunu 9 kolon yapar ve `ProductList` bileşenini render eder.
- `17`: `Render` fonksiyonunu ve JSX'i kapatır.
- `18`: Sınıf gövdesinin kapanışı.
- `20`: `Dashboard` bileşenini varsayılan olarak dışa aktarır.

### `Dosya: src/components/navi/Navi.js`

**Genel Görev:** Uygulamanın üst navigasyon barını ve sepet özetini gösterir.

**Kod Analizi:**
```javascript
import React, { useState } from 'react';
import CartSummary from '../cart/CartSummary';
import {
  Collapse,
  Navbar,
  NavbarToggler,
  NavbarBrand,
  Nav,
  NavItem,
  NavLink,
  NavbarText,
} from 'reactstrap';

function Example(args) {
  const [isOpen, setIsOpen] = useState(false);

  const toggle = () => setIsOpen(!isOpen);

  return (
    <div>
      <Navbar {...args}>
        <NavbarBrand href="/">reactstrap</NavbarBrand>
        <NavbarToggler onClick={toggle} />
        <Collapse isOpen={true} navbar>
          <Nav className="me-auto" navbar>
            <NavItem>
              <NavLink href="/components/">Components</NavLink>
            </NavItem>
            <NavItem>
              <NavLink href="https://github.com/reactstrap/reactstrap">
                GitHub
              </NavLink>
            </NavItem>
            <CartSummary />
          </Nav>
          <NavbarText>Simple Text</NavbarText>
        </Collapse>
      </Navbar>
    </div>
  );
}

export default Example;
```

**Satır Satır Açıklama:**
- `1`: React ve `useState` hook'u içe aktarılır.
- `2`: Sepet özet bileşeni `CartSummary` import edilir.
- `3-11`: Reactstrap'in navbar bileşenleri destructuring ile içe alınır.
- `13`: Fonksiyonel bileşen `Example` tanımlanır; Reactstrap dokümantasyonundaki varsayılan adı korunmuş.
- `14`: `isOpen` state'i ve `setIsOpen` güncelleyicisi `false` başlangıç değeriyle oluşturulur.
- `16`: `toggle` fonksiyonu, `isOpen` değerini tersine çevirir (ancak altta `Collapse` bileşeni sabit `true` aldığı için state kullanılmıyor).
- `18-34`: Navbar arayüzünü döndürür.
- `19`: Dış `div` sarıcı.
- `20`: `Navbar` bileşeni; dışarıdan iletilen props'ları yayar.
- `21`: `NavbarBrand` logoya tıklanınca ana sayfaya döner; örnek olarak `reactstrap` yazılmış.
- `22`: Mobil görünümde menüyü açıp kapatan `NavbarToggler`, `toggle` fonksiyonuna bağlanır.
- `23`: `Collapse` bileşeni; `isOpen={true}` sabit değeriyle menü hep açık tutuluyor, bu kodda `isOpen` state'i kullanılmıyor.
- `24-32`: `Nav` içinde menü öğeleri ve `CartSummary` bileşeni yerleştirilir.
- `25-27`: İlk `NavItem` dokümantasyon sayfasına giden linktir.
- `28-31`: İkinci `NavItem` GitHub reposuna yönlendirir.
- `32`: Sepet durumu özetini gösteren `CartSummary` bileşeni çağrılır.
- `33`: `NavbarText`, sağ tarafta basit bir metin gösterir.
- `36`: Fonksiyon sonu.
- `38`: Bileşen varsayılan olarak dışa aktarılır.

### `Dosya: src/components/cart/CartSummary.js`

**Genel Görev:** Navigasyon menüsünde sepetin mevcut durumunu gösterir; sepet boşken mesaj, doluyken dropdown listesi render eder.

**Kod Analizi:**
```javascript
import React from "react";
import { connect } from "react-redux";
import {
  UncontrolledDropdown,
  DropdownToggle,
  DropdownItem,
  DropdownMenu,
  NavItem,
  NavLink,
  Badge,
} from "reactstrap";
import { bindActionCreators } from "redux";
import * as cartActions from "../../redux/actions/cartActions";
import { Link } from "react-router-dom";

class CartSummary extends React.Component {
  renderEmpty() {
    return (
      <NavItem>
        <NavLink>Your cart is empty</NavLink>
      </NavItem>
    );
  }
  renderSummary() {
    return (
      <UncontrolledDropdown nav inNavbar>
        <DropdownToggle nav caret>
          Options
        </DropdownToggle>
        <DropdownMenu right>
          {this.props.cart.map((cartItem) => (
            <DropdownItem key={cartItem.product.id}>
              <Badge
                color="danger"
                onClick={() =>
                  this.props.actions.removeFromCart(cartItem.product)
                }
              >
                X
              </Badge>
              {cartItem.product.name}
              <Badge color="success" className="ml-2">
                {cartItem.quantity}
              </Badge>
            </DropdownItem>
          ))}
          <DropdownItem divider />
          <DropdownItem><Link to={"/cart"}>Go to Cart</Link></DropdownItem>
        </DropdownMenu>
      </UncontrolledDropdown>
    );
  }
  render() {
    return (
      <div>
        {this.props.cart.length > 0 ? this.renderSummary() : this.renderEmpty()}
      </div>
    );
  }
}

function mapDispatchToProps(dispatch) {
  return {
    actions: {
      removeFromCart: bindActionCreators(cartActions.removeFromCart, dispatch),
    },
  };
}

function mapStateToProps(state) {
  return {
    cart: state.cartReducer,
  };
}
export default connect(mapStateToProps, mapDispatchToProps)(CartSummary);
```

**Satır Satır Açıklama:**
- `1`: React sınıf bileşeni oluşturmak için temel kütüphane.
- `2`: Redux store verilerini bileşene bağlamak üzere `connect` fonksiyonu import edilir.
- `3-9`: Reactstrap'ten dropdown ve navigasyon bileşenleri alınır.
- `10`: Redux'un `bindActionCreators` fonksiyonu action yaratıcılarını dispatch'e bağlamak için çağrılır.
- `11`: Sepetle ilgili action creator'lar isim alanı olarak içe aktarılır.
- `12`: Router linkleri oluşturmak için `Link` bileşeni kullanılır.
- `14`: `CartSummary` sınıf bileşeni tanımlanır.
- `15-20`: Sepet boşsa gösterilecek `renderEmpty` metodu.
- `16-19`: Boş sepet için `NavItem` ve `NavLink` içeren JSX döndürür.
- `21-43`: Sepet doluysa dropdown menüsünü oluşturan `renderSummary` metodu.
- `23`: Navigasyon içinde serbest çalışan dropdown (`UncontrolledDropdown`) başlatılır.
- `24-26`: Dropdown başlığı `Options` olarak gösterilir.
- `27`: Menü içeriği için `DropdownMenu` açılır, `right` props'u içerik hizasını belirler.
- `28-38`: Sepetteki her ürün için `DropdownItem` render edilir.
- `29`: Liste öğesinin `key` prop'u ürün ID'sidir.
- `30-35`: Ürünü sepetten çıkarmak için kırmızı bir `Badge`; tıklanınca `removeFromCart` action'ı dispatch edilir.
- `36`: Ürünün adı gösterilir.
- `37-39`: Ürün miktarını yeşil `Badge` içinde yayınlar; `ml-2` sınıfı Bootstrap'te sol boşluk sağlar.
- `40`: Map döngüsü kapanır.
- `41`: Bölücü çizgi oluşturan `DropdownItem divider`.
- `42`: Sepet sayfasına yönlendiren `Link` bileşeni içeren `DropdownItem`.
- `45-49`: Ana `render` metodu; sepet doluysa `renderSummary`, değilse `renderEmpty` çağrılır.
- `46`: Dış sarmalayıcı `div`.
- `47`: Ternary operatör ile içerik seçimi.
- `52-56`: `mapDispatchToProps`; `removeFromCart` action'ını dispatch'e bağlar.
- `58-61`: `mapStateToProps`; store'daki `cartReducer` sonucunu `cart` prop'una eşler.
- `62`: `connect` fonksiyonu ile bileşen, Redux store'a bağlanarak dışa aktarılır.

### `Dosya: src/components/cart/CartDetail.js`

**Genel Görev:** Sepet sayfasında ürünleri tablo halinde listeler; kullanıcıların sepetten ürün çıkarmasına imkân verir.

**Kod Analizi:**
```javascript
import React from "react";
import { bindActionCreators } from "redux";
import * as cartActions from "../../redux/actions/cartActions";
import { connect } from "react-redux";
import { Table, Button } from "reactstrap";

class CartDetail extends React.Component {
  render() {
    return (
      <div>
        <Table>
          <thead>
            <tr>
              <th>#</th>
              <th>Name</th>
              <th>Price</th>
              <th>Quantity</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            {this.props.cart.map((cartItem) => (
              <tr key={cartItem.product.id}>
                <th scope="row">{cartItem.product.id}</th>
                <td>{cartItem.product.name}</td>
                <td>{cartItem.product.price}</td>
                <td>{cartItem.quantity}</td>
                <Button
                  color="danger"
                  onClick={() =>
                    this.props.actions.removeFromCart(cartItem.product)
                  }
                >
                  Sil
                </Button>
              </tr>
            ))}
          </tbody>
        </Table>
      </div>
    );
  }
}

function mapDispatchToProps(dispatch) {
  return {
    actions: {
      removeFromCart: bindActionCreators(cartActions.removeFromCart, dispatch),
    },
  };
}

function mapStateToProps(state) {
  return {
    cart: state.cartReducer,
  };
}
export default connect(mapStateToProps, mapDispatchToProps)(CartDetail);
```

**Satır Satır Açıklama:**
- `1`: React kütüphanesini içe aktarır.
- `2`: Redux'un `bindActionCreators` fonksiyonunu getirir.
- `3`: Sepet action creator'larını namespace olarak import eder.
- `4`: `connect` fonksiyonu ile Redux store'a bağlanmayı sağlar.
- `5`: Reactstrap'ten tablo ve buton bileşenlerini alır.
- `7`: `CartDetail` sınıf bileşeni tanımlanır.
- `8-36`: `render` metodu sepet tablo görünümünü oluşturur.
- `10`: Dış `div` wrapper.
- `11`: Bootstrap tablosu başlatılır.
- `12-18`: Tablo başlık satırı kolon adlarını tanımlar.
- `19-33`: `tbody` içinde sepet öğeleri map edilerek satırlar oluşturulur.
- `20`: Her `cartItem` için `key` prop'u ürün ID'sidir.
- `21-24`: Ürünün ID'si, adı, fiyatı ve miktarı sütunlara yerleştirilir.
- `25-31`: Sil butonu; kırmızı renkte render edilir ve tıklanınca `removeFromCart` dispatch eder.
- `37-41`: `mapDispatchToProps`; `removeFromCart` action'ını bağlar.
- `43-46`: `mapStateToProps`; store'daki `cartReducer` sonucunu `cart` prop'una map eder.
- `47`: `connect` ile bileşen Redux'a bağlanarak dışa aktarılır.

### `Dosya: src/components/categories/CategoryList.js`

**Genel Görev:** Kategori listesini yükler ve kullanıcı kategori seçtiğinde Redux state'ini günceller, ilgili ürünleri getirir.

**Kod Analizi:**
```javascript
import React from "react";
import { connect } from "react-redux";
import { bindActionCreators } from "redux";
import * as categoryActions from "../../redux/actions/categoryActions";
import { ListGroup, ListGroupItem } from "reactstrap";
import * as productActions from "../../redux/actions/productActions";

class CategoryList extends React.Component {
  componentDidMount() {
    this.props.actions.getCategories();
  }

  selectCategory = (category) => {
    this.props.actions.changeCategory(category);
    this.props.actions.getProducts(category.id);
  };

  render() {
    return (
      <div>
        <h3>Categories</h3>
        <ListGroup>
          {this.props.categories.map((category) => (
            <ListGroupItem
              active={category.id === this.props.currentCategory.id}
              onClick={() => this.selectCategory(category)}
              key={category.id}
            >
              {category.name}
            </ListGroupItem>
          ))}
        </ListGroup>
      </div>
    );
  }
}

function mapStateToProps(state) {
  return {
    currentCategory: state.changeCategoryReducer,
    categories: state.categoryListReducer,
  };
}

function mapDispatchToProps(dispatch) {
  return {
    actions: {
      getCategories: bindActionCreators(
        categoryActions.getCategories,
        dispatch
      ),
      changeCategory: bindActionCreators(
        categoryActions.changeCategory,
        dispatch
      ),
      getProducts: bindActionCreators(productActions.getProducts, dispatch),
    },
  };
}

export default connect(mapStateToProps, mapDispatchToProps)(CategoryList);
```

**Satır Satır Açıklama:**
- `1`: React temeli.
- `2`: Redux store verilerini bileşene bağlamak için `connect`.
- `3`: `bindActionCreators` kullanımı.
- `4`: Kategoriye ilişkin action creator'lar.
- `5`: Reactstrap'ten liste bileşenleri.
- `6`: Ürün action'larını (kategori seçildiğinde filtrelemek için) import eder.
- `8`: `CategoryList` sınıf bileşeni tanımlanır.
- `9-11`: `componentDidMount`, bileşen yüklendiğinde kategorileri API'den çeker.
- `13-16`: `selectCategory` arrow fonksiyonu; seçilen kategoriyi Redux state'ine yazar ve uygun ürünleri ister.
- `18-32`: Render metodu UI çıktısını verir.
- `20`: Başlık etiketi.
- `21-29`: `ListGroup` içinde kategoriler döngülenir.
- `23`: Sıradaki kategorinin, seçilen kategoriyle eşleşip eşleşmediğini kontrol ederek `active` durumunu belirler.
- `24`: Liste öğesine tıklandığında `selectCategory` çalıştırılır.
- `25`: React'in key props'u olarak kategori ID'si kullanılır.
- `26`: Kategori adı gösterilir.
- `34-38`: `mapStateToProps`; mevcut ve tüm kategorileri store'dan çeker.
- `40-53`: `mapDispatchToProps`; kategori ve ürünlerle ilgili action creator'ları dispatch'e bağlar.
- `54`: `connect` ile bileşeni sarmalar ve dışa aktarır.

### `Dosya: src/components/products/ProductList.js`

**Genel Görev:** Ürünleri tablo halinde gösterir, kategori seçimine göre filtrelenen ürünleri Redux üzerinden çeker ve sepet ekleme işlevi sağlar.

**Kod Analizi:**
```javascript
import React from "react";
import { connect } from "react-redux";
import { Badge, Button } from "reactstrap";
import { bindActionCreators } from "redux";
import { Table } from "reactstrap";
import alertify from "alertifyjs";
import * as productActions from "../../redux/actions/productActions";
import * as cartActions from "../../redux/actions/cartActions";

class ProductList extends React.Component {
  componentDidMount() {
    this.props.actions.getProducts();
  }

  addToCart = (product) => {
    this.props.actions.addToCart({ quantity: 1, product });
    alertify.success(product.name + " added to cart!");
  }

  render() {
    return (
      <div>
        <h3>
          <Badge color="success">{this.props.currentCategory.name}</Badge>
        </h3>
        <Table>
          <thead>
            <tr>
              <th>#</th>
              <th>Name</th>
              <th>Price</th>
              <th>In Stock?</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            {this.props.products.map((product) => (
              <tr key={product.id}>
                <th scope="row">{product.id}</th>
                <td>{product.name}</td>
                <td>{product.price}</td>
                <td>{product.inStock ? "Yes" : "No"}</td>
                <Button color="success" onClick={() => this.addToCart(product)}>Sepete Ekle</Button>
              </tr>
            ))}
          </tbody>
        </Table>
      </div>
    );
  }
}

function mapStateToProps(state) {
  return {
    currentCategory: state.changeCategoryReducer,
    products: state.productListReducer,
  };
}

function mapDispatchToProps(dispatch) {
  return {
    actions: {
      getProducts: bindActionCreators(productActions.getProducts, dispatch),
      addToCart: bindActionCreators(cartActions.addToCart, dispatch),
    },
  };
}

export default connect(mapStateToProps, mapDispatchToProps)(ProductList);
```

**Satır Satır Açıklama:**
- `1`: React importu.
- `2`: Redux store bağlantısı için `connect`.
- `3`: Reactstrap'ten `Badge` ve `Button` bileşenleri.
- `4`: `bindActionCreators`.
- `5`: Reactstrap `Table` bileşeni.
- `6`: Bildirim mesajları için `alertify` modülü.
- `7`: Ürün action creator'ları.
- `8`: Sepet action creator'ları.
- `10`: `ProductList` sınıf bileşeni tanımlanır.
- `11-13`: Bileşen mount olduğunda ürünleri API'den çeker.
- `15-18`: `addToCart` fonksiyonu; ürünü Redux sepetine ekler ve toast gösterir.
- `20-37`: Render metodu.
- `22-24`: Seçili kategori adını yeşil rozet içinde gösterir.
- `25-33`: Ürün tablosu başlığı ve gövdesi.
- `30-35`: Her ürün için tablo satırı oluşturur.
- `32`: Ürünün stoğa bağlı olarak `Yes/No` metni üretir.
- `33-34`: Sepete ekleme butonu, `addToCart` fonksiyonuna bağlanır.
- `39-43`: `mapStateToProps`; güncel kategori ve ürün listesini store'dan çeker.
- `45-50`: `mapDispatchToProps`; ürün çekme ve sepete ekleme action'larını dispatch'e bağlar.
- `52`: `connect` ile bileşeni dışa aktarır.

### `Dosya: src/redux/actions/actionTypes.js`

**Genel Görev:** Redux action'larında kullanılan string sabitlerini merkezi olarak tanımlar.

**Kod Analizi:**
```javascript
export const CHANCE_CATEGORY = 'CHANCE_CATEGORY';
export const GET_CATEGORIES_SUCCESS = 'GET_CATEGORIES_SUCCESS';
export const GET_PRODUCTS_SUCCESS = 'GET_PRODUCTS_SUCCESS';
export const ADD_TO_CART = 'ADD_TO_CART';
export const REMOVE_FROM_CART = 'REMOVE_FROM_CART';
```

**Satır Satır Açıklama:**
- `1`: Aktif kategoriyi değiştirmek için kullanılan action tipi sabitidir; isminde yazım hatası (`CHANCE` yerine `CHANGE`) olsa da uygulama boyunca tutarlı kullanıldığı için çalışır.
- `2`: Kategori listesinin API'den başarıyla alınması durumunda kullanılan action tipi.
- `3`: Ürün listesinin başarıyla alınmasında kullanılan action tipi.
- `4`: Sepete ürün ekleme işlemi için kullanılan action tipi.
- `5`: Sepetten ürün çıkarma işlemi için action tipi.

### `Dosya: src/redux/actions/cartActions.js`

**Genel Görev:** Sepetle ilgili Redux action creator fonksiyonlarını sağlar.

**Kod Analizi:**
```javascript
import * as actionTypes from './actionTypes';

export function addToCart(cartItem){
    return {type: actionTypes.ADD_TO_CART, payload: cartItem}
}

export function removeFromCart(product){
    return {type: actionTypes.REMOVE_FROM_CART, payload: product}
}
```

**Satır Satır Açıklama:**
- `1`: Action type sabitlerini namespace olarak import eder.
- `3-5`: `addToCart` fonksiyonu; verilen `cartItem` nesnesiyle `ADD_TO_CART` action'ı oluşturur.
- `7-9`: `removeFromCart` fonksiyonu; çıkarılacak `product` nesnesini payload olarak taşıyan `REMOVE_FROM_CART` action'ı üretir.

### `Dosya: src/redux/actions/categoryActions.js`

**Genel Görev:** Kategori ile ilgili action creator'ları tanımlar ve API'den kategori listesini çekmek için thunk fonksiyonu sağlar.

**Kod Analizi:**
```javascript
import * as actionTypes from './actionTypes';

export function changeCategory(category){
    return {type: actionTypes.CHANCE_CATEGORY, payload: category};
}

export function getCategoriesSuccess(categories){
    return {type: actionTypes.GET_CATEGORIES_SUCCESS, payload: categories};
}

export function getCategories(){
    return function(dispatch){
        let url = "https://test-api.nflu.dev/categories";
        return fetch(url)
        .then(response => response.json())
        .then(result => dispatch(getCategoriesSuccess(result)));
    }
}
```

**Satır Satır Açıklama:**
- `1`: Action type sabitlerini namespace olarak import eder.
- `3-5`: `changeCategory` fonksiyonu; seçilen kategoriyi payload olarak taşıyan action döndürür (tip adı `CHANCE_CATEGORY`).
- `7-9`: `getCategoriesSuccess`, API'den gelen kategori listesini payload olarak taşıyan success action'ıdır.
- `11-17`: `getCategories` thunk fonksiyonu; asenkron API çağrısı yapar.
- `12`: Thunk pattern'i gereği dispatch parametresi alan fonksiyon döndürür.
- `13`: Kullanılacak API endpoint'i tanımlar.
- `14`: Fetch API çağrısını başlatır.
- `15`: Yanıtı JSON'a çevirir.
- `16`: JSON sonucunu `getCategoriesSuccess` action'ı ile dispatch eder.
- `17`: Döndürülen promise ile zinciri sonlandırır.

### `Dosya: src/redux/actions/productActions.js`

**Genel Görev:** Ürünlerle ilgili action creator'ları sağlar ve isteğe bağlı kategori filtresiyle ürünleri API'den getiren thunk fonksiyonunu içerir.

**Kod Analizi:**
```javascript
import * as actionTypes from './actionTypes';

export const getProductsSuccess = (products) => ({
    type: actionTypes.GET_PRODUCTS_SUCCESS,
    payload: products,
});

export function getProducts(categoryId) {
    return function (dispatch) {
        let url = "https://test-api.nflu.dev/products";
        if (categoryId) {
            url += "?categoryId=" + categoryId;
        }
        return fetch(url)
            .then((response) => response.json())
            .then((data) => dispatch(getProductsSuccess(data)));
    }
}
```

**Satır Satır Açıklama:**
- `1`: Action type sabitlerini import eder.
- `3-6`: `getProductsSuccess` fonksiyonu başarılı ürün çekme işlemine ait action nesnesi üretir.
- `8-16`: `getProducts` thunk fonksiyonu.
- `9`: Dispatch fonksiyonunu parametre olarak alan iç fonksiyon döndürülür.
- `10`: Temel ürün API URL'sini belirler.
- `11-13`: Opsiyonel `categoryId` varsa URL'ye query string olarak ekler.
- `14`: Fetch çağrısı yapar.
- `15`: Yanıtı JSON'a dönüştürür.
- `16`: Sonucu `getProductsSuccess` action'ı ile dispatch eder.

### `Dosya: src/redux/reducers/initialState.js`

**Genel Görev:** Redux store'un başlangıç durumunu merkezi olarak tanımlar.

**Kod Analizi:**
```javascript
export default {
    currentCategory: {},
    categories: [],
    products: [],
    cart: []
}
```

**Satır Satır Açıklama:**
- `1-6`: Varsayılan state objesini dışa aktarır; seçili kategori boş nesne, kategoriler ve ürün listeleri boş diziler, sepet boş dizidir.

### `Dosya: src/redux/reducers/changeCategoryReducer.js`

**Genel Görev:** Seçili kategoriyi store'da tutan reducer'dır.

**Kod Analizi:**
```javascript
import * as actionTypes from '../actions/actionTypes';
import initialState from './initialState';

export default function changeCategoryReducer(state = initialState.currentCategory, action){
    switch (action.type) {
        case actionTypes.CHANCE_CATEGORY:
            return action.payload;
        default:
            return state;
    }
}
```

**Satır Satır Açıklama:**
- `1`: Action type sabitlerini import eder.
- `2`: Başlangıç state nesnesini alır.
- `4`: Reducer fonksiyonu tanımı; default state olarak `initialState.currentCategory` kullanılır.
- `5-9`: Action tipine göre state dönüşleri.
- `6`: `CHANCE_CATEGORY` action'ı geldiğinde payload doğrudan yeni state olur.
- `7-8`: Diğer tüm action'larda mevcut state döndürülür.

### `Dosya: src/redux/reducers/categoryListReducer.js`

**Genel Görev:** API'den çekilen kategori listesini Redux store'unda tutar.

**Kod Analizi:**
```javascript
import * as actionTypes from '../actions/actionTypes';
import initialState from './initialState';

export default function changeCategoryReducer(state = initialState.categories, action){
    switch (action.type) {
        case actionTypes.GET_CATEGORIES_SUCCESS:
            return action.payload;
        default:
            return state;
    }
}
```

**Satır Satır Açıklama:**
- `1`: Action type sabitleri.
- `2`: Başlangıç state nesnesi.
- `4`: Reducer fonksiyonu; default state boş kategori dizisidir.
- `5-9`: Action türlerini değerlendirir.
- `6`: `GET_CATEGORIES_SUCCESS` geldiğinde payload'daki dizi state yerine geçer.
- `7-8`: Diğer action'larda mevcut state korunur.

### `Dosya: src/redux/reducers/productListReducer.js`

**Genel Görev:** API'den getirilen ürün listesini Redux store'unda tutan reducer.

**Kod Analizi:**
```javascript
import * as actionTypes from '../actions/actionTypes';
import initialState from './initialState';

export default function productListReducer(state=initialState.products, action) {
    switch (action.type) {
        case actionTypes.GET_PRODUCTS_SUCCESS:
            return action.payload;
        default:
            return state;
    }
}
```

**Satır Satır Açıklama:**
- `1`: Action type sabitlerini import eder.
- `2`: Başlangıç state nesnesini alır.
- `4`: Reducer fonksiyonu; varsayılan state boş ürün dizisidir.
- `5-9`: Action'ın tipine göre state güncellenir.
- `6`: `GET_PRODUCTS_SUCCESS` action'ı geldiğinde ürün listesi payload ile güncellenir.
- `7-8`: Diğer action'lar için state olduğu gibi döner.

### `Dosya: src/redux/reducers/cartReducer.js`

**Genel Görev:** Sepet öğelerini yöneten reducer; ürün ekleme ve çıkarma işlemlerini gerçekleştirir.

**Kod Analizi:**
```javascript
import * as actionTypes from "../actions/actionTypes";
import initialState from "./initialState";

export default function cartReducer(state = initialState.cart, action) {
  switch (action.type) {
    case actionTypes.ADD_TO_CART:
      var addedItem = state.find(
        (c) => c.product.id === action.payload.product.id
      );
      if (addedItem) {
        var newState = state.map((cartItem) => {
          if (cartItem.product.id === action.payload.product.id) {
            return Object.assign({}, addedItem, {
              quantity: addedItem.quantity + 1,
            });
          }
          return cartItem;
        });
        return newState;
      }
      else {
        return [...state, { ...action.payload}];
      }
    case actionTypes.REMOVE_FROM_CART:
      const rfcState = state.filter(
        cartItem => cartItem.product.id !== action.payload.id
      );
      return rfcState;
    default:
      return state;
  }
}
```

**Satır Satır Açıklama:**
- `1`: Action type sabitlerini import eder.
- `2`: Başlangıç state'ini getirir.
- `4`: Reducer fonksiyonu; varsayılan state boş sepet dizisidir.
- `5-25`: `switch` ile action tiplerini inceler.
- `6-17`: `ADD_TO_CART` action'ı işlendiğinde aynı ürün daha önce eklenmiş mi kontrol edilir.
- `7-9`: Sepette aynı ID'ye sahip ürün aranır.
- `10-16`: Ürün bulunduysa, tüm sepet öğeleri map edilerek ilgili öğenin miktarı bir artırılır.
- `12-14`: `Object.assign` ile immutability korunarak yeni miktar hesaplanır.
- `18-20`: Ürün sepette yoksa mevcut state'e payload kopyası eklenir.
- `21-24`: `REMOVE_FROM_CART` action'ı, eşleşen ürün ID'sini filtreleyerek çıkarır.
- `25`: Filtrelenmiş state döndürülür.
- `26-27`: Bilinmeyen action tiplerinde mevcut state aynen korunur.

### `Dosya: src/redux/reducers/index.js`

**Genel Görev:** Uygulamanın tüm reducer'larını birleştirerek kök reducer'ı oluşturur ve global stilleri import eder.

**Kod Analizi:**
```javascript
import { combineReducers } from "redux";
import changeCategoryReducer from "./changeCategoryReducer";
import categoryListReducer from "./categoryListReducer";
import productListReducer from "./productListReducer";
import cartReducer from "./cartReducer";
import 'bootstrap/dist/css/bootstrap.min.css';
import 'alertifyjs/build/css/alertify.min.css';

const rootReducer = combineReducers({
    changeCategoryReducer,
    categoryListReducer,
    productListReducer,
    cartReducer
});

export default rootReducer
```

**Satır Satır Açıklama:**
- `1`: Redux'un `combineReducers` yardımcı fonksiyonu import edilir.
- `2-5`: Uygulamadaki spesifik reducer modülleri içe aktarılır.
- `6-7`: Bootstrap ve Alertify CSS dosyaları global olarak projeye dahil edilir (genelde `index.js` içerisinde yapılır; burada reducer dosyasında olması dikkat çekicidir).
- `9-13`: Reducer'lar `combineReducers` ile tek nesnede birleştirilir.
- `15`: Birleştirilen reducer varsayılan olarak dışa aktarılır.

### `Dosya: src/redux/reducers/configureStore.js`

**Genel Görev:** Redux store'unu oluşturup `redux-thunk` ara katmanını ekler.

**Kod Analizi:**
```javascript
import { legacy_createStore, applyMiddleware } from "redux";
import rootReducer from "./index";
import { thunk } from "redux-thunk";

export default function configureStore(){
    return legacy_createStore(rootReducer, applyMiddleware(thunk));
}
```

**Satır Satır Açıklama:**
- `1`: Redux'un `legacy_createStore` (Redux Toolkit öncesi API) ve `applyMiddleware` fonksiyonları import edilir.
- `2`: Birleştirilen `rootReducer` içe aktarılır.
- `3`: `redux-thunk` paketinden `thunk` middleware'i import edilir.
- `5-6`: `configureStore` fonksiyonu store örneği döndürür; `thunk` middleware'i eklenmiştir, böylece action creator'lar fonksiyon döndürebilir.

## 4. Redux Veri Akışı ve Bileşen Etkileşimi
- Kullanıcı bir kategori seçtiğinde `CategoryList` bileşeni `changeCategory` ve `getProducts` action'larını tetikler. Bu action'lar sırasıyla `changeCategoryReducer` ve `productListReducer` tarafından işlenir; yeni kategori ve ürün listesi store'a yazılır.
- `ProductList` bileşeni store'dan gelen güncel `currentCategory` ve `products` prop'larıyla yeniden render edilir. `addToCart` action'ı sepete ürün eklerken `cartReducer` miktarları yönetir.
- Navigasyondaki `CartSummary` bileşeni `cartReducer` state'ini dinler; sepet değiştikçe dropdown içeriği güncellenir ve `CartDetail` sayfası ile senkron kalır.
- `redux-thunk` sayesinde `getCategories` ve `getProducts` gibi fonksiyonlar asenkron `fetch` çağrılarını dispatch öncesi çalıştırabilir.

## 5. İyileştirme Fikirleri
- Action type sabitinde yazım hatası bulunan `CHANCE_CATEGORY` anahtarını `CHANGE_CATEGORY` olarak güncellemek, kod okunabilirliğini artıracaktır.
- `Navi` bileşeninde `Collapse` bileşeni `isOpen` state'ine bağlanmadığı için menü sürekli açık kalıyor; `Collapse isOpen={isOpen}` şeklinde düzeltilmeli.
- `CartDetail` ve `ProductList` bileşenlerinde `Button` bileşeni `tr` içinde yer alıyor; semantik olarak `td` içine alınması daha doğru olur.
- CSS import'larının reducer dosyası yerine giriş noktasında (`src/index.js`) tutulması daha iyi bir ayrıştırma sağlar.

