# Critical Rendering Path

## Critical Rendering Path (CRP) Nedir?

Browser'in bir sayfayi render etmesinden baslayip, sayfanin tamamen yuklenmesine kadar gecen sureye `Critical Rendering Path (CRP)` denir.

Critical Rendering Path sirali olarak kisaca su adimlardan olusur:

1. DOM agacini olustur
2. CSSOM (CSS Object Model) agacini olustur
3. JavaScript'i calistir
4. Render tree'yi olustur
5. Layout'u olustur
6. Paint

Yukaridaki adimlarin detayli aciklamasi ise soyle:

1. Browser tarafindan yuklenen HTML parse edilip DOM agaci olusturulur.
2. Browser tarafindan yuklenen CSS dosyasi parse edilip CSSOM agaci olusturulur.
   - Not: CSS render-blocking oldugu icin parse edilmeden once Render tree olusturulmaz. [Buraya](performans.md) gozatmanizi oneririm.
3. JavaScript, parseri blockladigi icin HTML dokumani `<script>` tagini gorene kadar blocklanmis olur.
   > - Not: Bu yuzden `<script>` tagini HTML dosyalarinin sonuna koyuyoruz. Eger dosyanin sonuna koymak istemezseniz, `<script async src="script.js">` olarak JS dosyasini asenkron sekilde yukleyebilirsiniz.
4. Ikinci adimda parse edilen CSS dosyasindaki selectorler birinci adimda parse edilen HTML nodelari ile karsilastirilarak stiller gerekli yerlere yerlestirilip birbirine baglanmis olur.
   > - Not: `display: none` propertysini kullandiginiz elementler render tree'de bulunmayacaktir.
5. Layout'u olusturma adiminda ise o anki viewport boyutuna gore elementlerin sekli ve pozisyonu belirlenir.
   > - Not: HTML dosyalarinin basina yazdigimiz `<meta name="viewport" content="width=device-width,initial-scale=1">` meta tagi ile viewport boyutunu belirliyoruz.
6. Bu adimda browser her elementin pixellerini katmanlara (layer) dolduracak ve sayfa bize gozukmus olacaktir.  

## CRP Suresi Nasil Azaltilir?

### Dosyalari minify etmek

HTML, CSS ve JavaScript dosyalarinizi minify ederek dosya boyutlarini kucultebilirsiniz.

### CSS Dosyalarini Kosullu Olarak Indirmek

Browserlar CSS'i parse edene kadar sayfanin render edilmesini durdurdugu (render-blocking) icin CSS dosyalarini kosullu olarak indirmek performans kazandirir.

```html
<link rel="stylesheet" href="styles.css" />
<!-- render blockluyor -->
<link rel="stylesheet" href="print.css" media="print" />
<link
  rel="stylesheet"
  href="mobile.css"
  media="screen and (max-width: 480px)"
/>
<!-- 480px ve ustu ekranlar icin renderi blocklamiyor -->
```

> Not: Aslinda browserlar yine de ilgili CSS dosyasini indiriyor fakat CSS dosyasinin parse edilmesini beklemeden sayfayi render ediyor. (non render-blocking)

<br/>

### CSS Dosyalarini Asenkron Olarak Indirmek

```html
<link
  rel="stylesheet"
  href="style.css"
  media="print"
  onload="this.media='all'"
/>
```

Yukarida HTML icine `link` ile ekledigimiz CSS dosyasini sadece media tipi [print](https://www.w3schools.com/tags/att_link_media.asp) iken yuklemesini soyluyoruz. Yani aslinda bizim sayfamiz acilirken hicbir zaman media tipi print olmayacagi icin ve yukarida da bahsettigim gibi CSS dosyasi her halukarda indirilecegi icin ekledigimiz CSS dosyasi sayfanin acilmasini blocklamiyor. Sayfanin render edilmesi bittiginde de media tipini `all` yapip butun cihazlar icin CSS'i aktif hale getiriyoruz.

> Burada media tipini rastgele bir sey degil de print yapmamizin nedeni [bazi](https://bugs.chromium.org/p/chromium/issues/detail?id=977573) browserlarin gecersiz media tiplerini desteklemeyi kaldirabilme ihtimali olmasidir. (Normalde gecersiz girilse de ilgili CSS dosyasi browser tarafindan indiriliyor)

### JavaScript'i `async` veya `defer` kullanarak indirmek



- `async`: JS dosyasi asenkron olarak indirilir ve dosya indigi an execute edilir.

Dosyalarinizin yapisina bagli olmakla birlikte JavaScript'i async olarak yuklemek icin:

```html
   <script async src="index.js"></script>
```

> Not: `async`'i sadece bir script'in baska bir script'e bagli olmadigi durumlarda kullanin.

- `defer`: JS dosyasi yine asenkron olarak indirilir fakat butun dokuman'in parse islemi bittiginde calisir. `defer` ile indirilen dosyalar dokumandaki siraya gore calisir.

JS dosyasini `defer` kullanarak yuklemek icin:

```html
   <script defer src="index.js"></script>
```


> Not: `defer`'i bir script'in baska bir script'e bagli oldugu durumlarda kullanmaniz gerekiyor. Ornegin jQuery'e bagli olan baska bir library kullaniyorsaniz ilk once jQuery'i yuklemeniz gerekiyor ki sorun olmasin.

### Lazy load

Butun resimlerin sayfa acilirken yuklenmesini istemiyorsaniz genellikle `img` taglerine `loading=lazy` attributesini vererek lazy loading yapmayi basarabilirsiniz:

```html
   <img loading="lazy" src="image.png" />
```

[Buradan](https://caniuse.com/loading-lazy-attr) kullanilabilirlik durumunu inceleyebilirsiniz.

