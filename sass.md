# SASS

Burada SASS'in ne oldugu ve ne ise yaradigiyla ilgili degil de SASS'i nasil daha cok surdurulebilir ve efektif kullanabilecegimizi anlatmaya calisacagim.

## Degisken Kullanmak

SASS'da degisken kullanmak icin herhangi bir CSS degerini `$` ile baslayan herhangi bir degisken ismine atayabilirsiniz:

```css
$primary-color: blue;
$border-color: gray;
```

Daha sonra bu degiskenleri su sekilde kullanabilirsiniz:

```css
  .input {
    color: $primary-color;
    border: 1px solid $border-color;
  }
```

### Degiskenlerin Scope'u

Eger SASS degiskenlerini dosyanin en uzerinde tanimliyorsaniz, bu degiskenlere global olarak herhangi bir yerden ulasabilirsiniz:

```css
$global-degisken: global deger;
```

Eger degiskeni bir block icerisinde tanimlamissiniz o degisken'in scope'u localdir:

```css
.app {
  $local-degisken: local deger;
}
```

### CSS'deki `var` ile SASS'daki `variable` Arasindaki Farklar

`var`: 
- CSS'de `var` ile tanimladigimiz bir degiskenin degeri degistiginde browser gereken yerleri re-paint eder.
- `var` JavaScript ile manipule edilebilir.

`variable`: 
- SASS'da kullandigimiz `variable`'larin degerini degistirdigimizde, o variable'i onceden kullandigimiz yerler ayni kalir.
  > - Not: Ornegin 2 temali bir websitesi yapmak istersek her ikisi icin de ayri variable'lar olusturmak zorundayiz. 
- SASS'da tanimladiginiz degiskenler compile edildiginde variable ismiyle variable degeri yer degistirir.
  > - Daha iyi aciklamak gerekirse soyle:
    - ```css 
      $primary: blue;

      /* SCSS */
      .button {
        color: $primary;
      }

      /* CSS */

      .button {
        color: blue;
      }
      ```
## Parent (`&`) Selector Kullanmak

SASS'da `&` selectoru bir element'in parentina isaret eder:

```css
.navbar {
  /* Burada aslinda navbar classinin child'i olan navbar-container'i sectik */
  &-container {
  }
}

button {
  /* button:hover */
  &:hover {
  }
}

.box {
  :not(&) {
  }
}
```

## Placeholder Selectorler

SASS'da ayni class selectorleri gibi calisan placeholder selectorleri basina `%` konarak tanimlanir ve bu selectorler cikartilan CSS dosyasinda yer almaz (Siz herhangi bir yerde extend edene kadar private olarak kalir) fakat bu selectorleri extend edebilirsiniz:

```css
%button {
  background-color: blue;
  padding: 12px;

  &:hover {
    background-color: pink;
  }
}

.big-button {
  @extend %button;

  width: 150px;
  height: 150px;
}

.small-button {
  @extend %button;

  width: 40px;
  height: 40px;
}
```

## Utility Classlar Icin `maps` ve `@each` Kullanmak

Utility classlarinizi yazarken hepsini tek tek su sekilde yazmak yerine:

```css
.mt-4 {}
.mt-8 {}
.mt-12 {}
.mt-16 {}

.mb-4 {}
.mb-8 {}
.mb-12 {}
    .
    .
    .
```

Soyle yapabilirsiniz:

```css
$spacers: ("4": 4px, "8": 8px, "12": 12px, "16": 16px);

@each $name, $value in $spacers {
  .mt-#{$name} {
    margin-top: $value;
  }

  .mb-#{$name} {
    margin-bottom: $value;
  }

  .ml-#{$name} {
    margin-left: $value;
  }

  .mr-#{$name} {
    margin-right: $value;
  }

  .pt-#{$name} {
    padding-top: $value;
  }

          .
          .
          .
}
```

Bu sekilde yazdigimiz kod daha okunabilir ve maintain edilebilir olur.

## `@extend`

`@extend` at-rule'unu daha once tanimladiginiz bir classin stillerini baska bir class icin de kullanmak istediginiz yerlerde kullanabilirsiniz:

```css
.box {
  color: $primary;
  padding: 20px;
  border: 1px solid $border-color;
}

.box--big {
  @extend .box;
  border-width: 5px;
}
```

## `@mixin` ve `@include`

SASS'da mixinler daha sonra kullanabileceginiz yapilar kurmanizi saglar.

Mixinler `@mixin` at-rule'u ile tanimlanir ve parametre alabilirler:

```css
@mixin button {
  padding: 12px;
  border: none;
}

/* Burada $padding parametresine default olarak 20px verdik. */
@mixin box($bg-color, $padding: 20px) {
  background-color: $bg-color;
  padding: $padding;
}
```

Yazdigimiz mixinleri herhangi bir yerde kullanmak icin `@include` at-rule'unu kullanmamiz gerekiyor:

```css
.button {
  @include button;
}

.box {
  @include box($bg-color: blue);
}
```

Mixinlere sonsuz sayida parametre de gecirebiliriz:

```css
/* $box-selectors parametresinin sonundaki 3 noktaya dikkat edin */
@mixin colorize-boxes($color, $box-selectors...) {
  @for $i from 1 to length($box-selectors) {
    #{nth($selectors, $i)} {
      background-color: $color;
    }
  }
}

@include colorize-boxes(red, "box-first", "box-second", "box-third");
```

> Not: Yukaridaki kisimda box-selectors parametresi [`list`](https://sass-lang.com/documentation/values/lists) olarak geldigi icin `length` ve `nth` fonksiyonlarini kullanabiliyoruz.

## `@content`

SASS'da `@content` at-rule'u mixinlerin icerisinde tanimlanabiliyor olup bir block'un butun iceriginin sonradan doldurulacagini belirtmemize yarar:

```css
@mixin screen-mw($width) {
  @media screen and (max-width: $width) {
    @content;
  }
}

@include screen-mw(768px) {
  ...
}
```

Yukarida gordugunuz gibi `@content`'i kullanarak oradaki block'un daha sonra doldurulacagini belirttik.

## `@function`

SASS'da fonksiyonlar `@function` at-rule'u ile tanimlanir ve kompleks yapilarin insa edilmesinde onemli rol oynar.

Ornegin `px` unitlerin `em`'e cevrilmesi icin bir function yazabiliriz:

```css
  @function px-to-em($px, $base-px: 16) {
    @return #{$px / $base-px}em; /* `@return` at-rule'u ile istedigimiz degeri donduruyoruz */
  }
```

Daha sonra bunu istedigimiz yerde kullanabiliriz:

```css
  .text--big {
    font-size: px-to-em(52)
  }
```
