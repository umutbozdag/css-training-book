# CSS ve performans

## Selectorler

CSS selectorleri sagdan sola dogru okundugu icin selectorleri nasil yazdiginiz performans acisindan buyuk onem tasir.

<br/>

### ID

HTML dosyasindaki kullandiginiz ID'ler unique olmasi gerektigi icin CSS dosyasi parse edilirken o ID'ye ait node'un da hizlica bulunmasi beklenir.

Ornegin: `#navbar`

> Not: ID'lerin ne kadar en hizli olan selector cesidi oldugu bilinse de, bu en hizli olma durumu sadece soz konusu ID `key selector` ise gecerli olabilir.

> `Key selector`: Coklu selectorlerde bulunan en sagdaki stil verilmek istenilen selector. Selectorlerin performansini en cok etkileyecek olan seylerden biri

- Ornegin: `#navbar .container` selectorundeki key selector `container` classidir.

> Ipucu: `#main .images-list li img` selectorundeki `img` tagine ornegin `list-img` classi verip direkt `.list-img` olarak secerseniz performans kaybi olmaz.

> Not: Browserlar HTML dosyasinda kullandiginiz ID'lerin unique olup olmayacagini (veya var olup olmayacagini) tam olarak bilemedigi icin CSS dosyasindaki selectorleri sagdan sola dogru okumaya baslar ki eger en sagdaki selector'un (key selector) karsiligi HTML dosyasinda bulunamiyorsa direkt o selectorle ugrasmayi birakirlar. Eger soldan saga olsaydi en sagdaki selectore gelene kadar butun selectorleri okuyup HTML dosyasiyla karsilastirma yapilmak zorunda kalinacakti ki bu da performans acisindan kotu bir sey.

### Class

ID ve Class selectorleri arasindaki performans farki neredeyse [yok denecek kadar az oldugu](https://stackoverflow.com/a/31027148/10991790) ve [burada](surdurulebilirlik-ve-best-practiceler.md) da bahsettigimiz gibi ID selectorlerini specificity acisindan kullanmamamiz gerektigi icin class selectorlerini kullanmak daha mantiklidir.

### Universal selector (*)

Universal selector butun her seyi sececegi icin bu selectorden ne kadar kacinirsaniz performans acisindan o kadar iyi olur.

<br/>

## will-change

CSS'de `will-change` propertysi bir HTML elementinin hangi ozelliginin degisecegini onceden browsera haber vermemize ve browserin bu bilgiye gore gereken yerlere onceden optimizasyon iyilestirmeleri yapmasini saglar.

> Not: Bu propertyi kullanirken dikkatli olmalisiniz cunku aksiyonunuz baslamadan hemen once (ornegin opacityi degistirme) kullanirsaniz, browserin optimizasyon yapma suresi yetismeyecegi icin performans kaybi yasarsiniz. Ornegin:

```css
.button:hover {
  will-change: opacity;
  opacity: 0.5;
}
```

Yukaridaki gibi direkt button hover ediliyorken will-change'i kullanmak yerine,

```css
.buttons-parent:hover .button {
  will-change: opacity;
}
.button:hover {
  opacity: 0.5;
}
```

button'un parent elementine will-change uygularsak, browsera yeteri kadar zamani tanimis olup optimizasyon yapmasina olanak saglayabiliriz.

> Not: `will-change` icin birden fazla value kullanabilirsiniz:
- ```css
    .buttons-parent:hover .button { 
      will-change: transform, opacity;
    }
  ```
  

### Daha iyi bir yol

will-change'in isi bittikten sonra bunu kaldirmamiz daha iyi performans almamiza olanak saglar.

#### Ama nasil?

Eger will-change'i CSS icerisinde declare ederseniz isi bittiginde silemezsiniz fakat direkt olarak JavaScript icerisinde declare ederseniz bunu basarabilirsiniz.

Ornegin:

```js
const buttonsParentEl = document.querySelector("buttons-parent");

buttonsParentEl.addEventListener("mouseenter", addWillChange);
buttonsParentEl.addEventListener("mouseleave", removeWillChange);

function addWillChange() {
  this.style.willChange = "opacity";
}

function removeWillChange() {
  this.style.willChange = "auto";
}
```

## Animasyonlar ve Transitionlar

Browser bir sayfayi render ederken iki tane thread uzerinde islem yapar:

1. Main thread: DOM, CSSOM, Render tree'nin olusturulmasi, layout ve painting islemleri, JavaScript'in calismasi gibi islemler burada gerceklesir. Bu thread CPU'yu daha yogun bicimde kullanir.

2. Compositor thread: Ekrana bir seylerin cizilmesi, sayfada nerelerin gorunur oldugunu veya gorunur olacagini, sayfayi scroll ettiginizde ekrana nelerin cizilecegini belirleme gibi islemler yapar. Bu thread ise GPU'yu daha yogun bicimde kullanir.

Main threadi fazla yorarsaniz sayfanin responsive olmasi kacinilmazdir cunku main thread'in daha fazla memory ve zamana ihtiyaci vardir.

Iste bu yuzden `animation` ve `transitionlari` compositor thread'i etkileyecek sekilde kullanmamiz lazim.

### Peki bunu nasil yapmayiz?

`animation` ve `transition` kullanirken bir elemente layout ve painting'i re-calculate ettirecek propertyler kullanmamaliyiz (yani main thread'i etkilememeliyiz).

Ornegin:

```css
.box {
  height: 500px;
  width: 100px;

  left: -500px;
  transition: left 500ms linear;
}

.box-open .box {
  left: 0px;
  transition: left 500ms linear;
}
```

Yukaridaki ornekte `left` propertysi main thread'i etkileyip layout'u tekrar calculate ettirmek zorunda birakiyor. Bu yuzden `top/bottom/left/right` gibi propertyleri kullanmaktan kacinmaliyiz.

> Not: Hangi propertynin hangi thread'i etkiledigini [buradan](https://docs.google.com/spreadsheets/u/0/d/1Hvi0nu2wG3oQ51XRHtMv-A_ZlidnwUYwgQsPQUg1R2s/pub?single=true&gid=0&output=html) daha detayli bicimde inceleyebilirsiniz.

### Peki bunu nasil yapariz?

Bir elemente `animation` veya `transition` uygularken baslica:

- `transform: translateX(n) translateY(n) translateZ(n);`
- `transform: scale(n);`
- `transform: rotate(ndeg);`
- `opacity: n;`

Bu yukaridaki 4 propertyi kullanmaya ozen gostermeliyiz cunku bu propertyler direkt olarak compositor thread'i etkiler.

Yukaridaki ornegin dogru hali:

```css
.dropdown {
  transform: translateX(-100%);
  transition: transform 500ms linear;
}
.dropdown-open .dropdown {
  transform: translateX(0%);
  transition: transform 500ms linear;
}
```

> Ipucu: Daha once gordugumuz `will-change` propertysini `.dropdown` selectoru icin kullanirsak daha da performans kazanmis oluruz. Yani soyle:

```css
.dropdown {
  transform: translateX(-100%);
  transition: transform 500ms linear;

  will-change: transform;
}
```

<br/>

## Frameworkler ve List Rendering Performansi

Frameworkler'de listleri render ederken `key` (veya benzeri) prop'unu kullanmazsak listeye her yeni bir sey eklendiginde butun liste re-render edilir. (Bunu Chrome dev tools icin `More tools > Rendering > Paint flashing`'i aktif edip deneyebilirsiniz.)

### Peki bunu nasil cozeriz?

Cogu frameworklerde bulunan `key` prop'u (veya benzeri) ve data'nin degisebilme ihtimali oldugu yerlerde yukarida bahsettigimiz gibi `will-change` propertysi kullanilarak cozulebilir.

> Ornegin [buradaki](https://akryum.github.io/vue-virtual-scroller/#/dynamic) ornekte scrollbari asagiya cektigimizde gozle gorulur bir yavaslama oluyor. Cunku `vue-recycle-scroller` classinin oldugu `div` elementine `will-change` propertysi uygulanmamis. `will-change: transform` ekledikten sonra scroll ederken yavaslama tamamen kayboluyor. (Bunu `will-change` eklemeden once `Rendering` tab'i altindan `Scrolling performance issues`'i aktif ederseniz listenin sol ust kisminda `repaints on scroll` uyarisi uzerinden gorebilirsiniz)