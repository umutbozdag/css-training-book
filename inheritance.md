# Inheritance

CSS'de Inheritance (Kalitsallik) bir parent element'in child elementlerine property ozelliklerini gecirmesine denir.

Ornegin sunu dusunelim:

```html
<p>Paragraf yazisi <strong>Kalin yazi</strong> devam</p>
```

```css
p {
  color: red;
}
```

Burada `strong` elementi `p` elementinin child elementi oldugu icin `p` elementine verdigimiz `color` propertysi direkt olarak strong icin inherit ediliyor.

Fakat CSS'de her property parent elementten child elemente inherit edilmiyor.

Ornegin:

```css
p {
  border: 1px solid gray;
}
```

Yukarida `p` elementine verdigimiz `border` propertysi `strong` elementi icin gecerli olmaz. Bunu ancak kendimiz belirtirsek yapabiliriz:

```css
p {
  border: 1px solid gray;
}

strong {
  border: inherit;
}
```

Bu ornegi canli olarak [buradan](https://codepen.io/umutbozdag/pen/yLOdPWv) inceleyebilirsiniz.

> Not: `background-color` inherit edilmeyen bir property fakat bu property'nin initial valuesi `transparent` oldugu icin child element'in arkaplan rengi de parentiyla ayni oluyor.

> Not: [Buradan](https://www.w3.org/TR/CSS21/propidx.html) butun inherit edilen ve edilmeyen propertyleri inceleyebilirsiniz.
