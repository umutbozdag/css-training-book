# Box model nedir?

Aslinda butun browserlar her bir elementi bir [kutu](https://codinglead.github.io/images/box-model.png) olarak ele alir. Bir box model'i 4 parcaya ayirabiliriz, Bunlar:

## Margin area

Margin area olarak adlandirilan kisim kutunun en disindaki kisim yani bizim kutumuzun diger kutularla arasindaki boslugu temsil ediyor diyebiliriz

## Border area

Border area olarak adlandirilan kisim ise margin ve padding areasinin arasinda kalan kisimdir. Yani kutunun etrafindaki siniri temsil ediyor diyebiliriz

## Padding area

Padding area olarak adlandirilan kisim bizim kutumuzun content area ile border area arasindaki boslugunu temsil eder

## Content area

Content area kutumuzun icindeki icerikleri temsil eder. (Yazi, resim gibi)

Ornegin:

```html
<button>Bu benim butonum</button>
```

```css
button {
  padding: 20px;
  border: 1px solid brown;
  margin: 10px;
}
```

- button elementine verdigimiz `10 piksellik margin` bizim kutumuzun diger komsu elementler ile arasindaki 10 piksel boslugu temsil eder

- button elementine verdigimiz `20 piksellik padding` bizim kutumuzun content ve border areasi arasindaki 20 piksel boslugu temsil eder

- button elementine verdigimiz `border` bizim kutumuzun siniri etrafinda 1 piksellik kahverengi renginde bir siniri temsil eder

Canli ornegi [buradan](https://codepen.io/umutbozdag/pen/vYGMzJe) inceleyebilirsiniz. Devtools'u acip Elements -> Computed altinda yukarida bahsettigim arealari cok rahat bicimde gorebilirsiniz.

## Box sizing

CSS'deki `box-sizing` ozelligi sizin kutunuzun yapisini belirler.

`box-sizing` ozelliginin alabilecegi 2 adet deger vardir.

1. `content-box`: Kutunuzun yuksekligi ve genisligi sadece icerik alanini icerir. (Yani kutunun boyutu `width` ve `height`'in uzerine `padding` ve `border` eklenerek hesaplanir)

Ornegin:

```css
.kutu {
  box-sizing: content-box;
  width: 200px;
  padding: 10px;
  border: 5px solid magenta;
}
```

Yukaridaki `kutu` classinin genisligi `200px` olmasi beklenirken `box-sizing` ozelliginin `content-box` olmasi nedeniyle genislik `width + padding + border genisligi` olarak hesaplanir. Yani bu durumda `200px genislik + sagdan soldan 10px padding + sagdan soldan 5px border genisligi = 230px` olur.

1. `border-box`: Kutunuzun genislik ve yukseklik degeri border ve padding degerlerini de icerir.

```css
  .kutu {
    box-sizing: border-box;
    width: 200px;
    padding: 10px;
    border: 5px solid magenta;
  }
```

Yukaridaki 2 ornegi de canli olarak [buradan](https://codepen.io/umutbozdag/pen/dyMxNNq) inceleyebilirsiniz.

