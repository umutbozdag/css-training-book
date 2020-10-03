# Animasyonlar

## Kisaca CSS animasyonu nedir?

CSS'de animasyonlar, bir elementin stilinin bir durumdan baska bir duruma gecmesi denilebilir.

## transition ile animation arasindaki farklar nelerdir?

- `transition` ve `animation` arasindaki en temel fark `transition` bir kere calisirken `animation` surekli olarak (veya istediginiz kadar) loop mantigiyla calisabilir.
- `animation` ile daha kompleks isler yapilabilir.
- `transition` sadece 2 farkli state arasinda degisebilirken (from-to, 0%-100%), `animation` cok daha fazla state arasinda degisiklik gosterebilir.

> Not: Eger yapacaginiz animasyon sadece 2 state arasindaysa ve basitse performans acisindan `transition` kullanmak cok daha mantiklidir. 

## Nasil kullanilir?

CSS'de animasyonlar `@keyframes` at-rule'i ile birlikte tanimlanir.

Ornegin:

```css
@keyframes test {
  from {
    background-color: yellow;
  }

  to {
    background-color: black;
  }
}
```

Simdi yukarida yazdigimiz `test` isimli animasyonumuzu bir element icin kullanalim:

> Not: `from` baslangici (0%), `to` ise bitisi ifade ediyor (100%)

```css
.box {
  width: 300px;
  height: 100px;
  background-color: red;
  animation-name: test;
  animation-duration: 4s;
}
```

[Burada](https://codepen.io/umutbozdag/pen/WNwBqvw) tahmin edebildiginiz gibi `animation-name` animasyonun ismi, `animation-duration` kac saniye surecegi.

CSS'de animasyon ile ilgili olan propertyler ise soyle:

- `animation-name`: Animasyonun ismi
- `animation-duration`: Animasyonun her bir seferde kac saniye surecegi
- `animation-timing-function`: Animasyonun zamanlama fonksiyonunu belirler
- `animation-delay`: Animasyonun baslamadan once ne kadar bekletilecegini belirler
- `animation-direction`: Animasyonun yonunu belirler
- `animation-iteration-count`: Animasyonun tekrar sayisini belirler
- `animation-fill-mode`: Animasyondan once veya sonra hangi degerlerin uygulanacagini belirler
- `animation-play-state`: Animasyonu durdurup/baslatmaya yarar

Shorthand olarak ise su sekilde kullanabiliriz: 

```css
@keyframes example {

}

.shorthand {
   animation: 
    example /* animation-name */
    1s /* animation-duration */
    ease /* animation-timing-function */
    0s /* animation-delay */
    alternate /* animation-direction */
    infinite /* animation-iteration-count */
    none /* animation-fill-mode */
    running /* animation-play-state */
}
```

Biraz daha karmasik bir ornek:

```html
<div class="box"></div>
```

```css
@keyframes bubble {
  0%,
  50% {
    transform: scale(5);
    transform: skew(50deg);
  }

  75% {
    opacity: 0.5;
  }

  100% {
    background-color: black;
    transform: scale(2);
  }
}

.box {
  height: 50px;
  width: 50px;
  margin: 0 auto;
  margin-top: 2rem;
  border-radius: 50%;
  background-color: red;
  animation: bubble 1s linear 0s alternate infinite none running;
}
```

Canli olarak [buradan](https://codepen.io/umutbozdag/pen/jOqgORw) inceleyebilirsiniz.

Animasyonlar'da performans icin [burayi](performans.md) okuyabilirsiniz