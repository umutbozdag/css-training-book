# Surdurulebilirlik ve Best Practiceler

> Not: Bu dosya'nin asil yazilma amaci ileride yapacagim projeler icin bir checklist gorevi gorecek olmasidir.

- CSS dosyani birden fazla dosyaya parcala
- CSS dosyasinin en tepesinde `icindekiler` kismi yazilabilir
- Her buyuk sectionun ustune comment olarak o sectionun basligini yaz
  > Not: section basligini # ile baslatirsan ararken daha kolay bulabilirsin
- Birbiriyle alakali olan selectorleri ayni satirda, alakasiz olanlari farkli satirda yaz
- BEM kullanilabilir
  - Block -> `sidebar`
  - Element -> `sidebar__menu`
  - Modifier -> `sidebar--active`
- Multi-line CSS'i ilgili selector sadece bir property kullaniyorsa ve birbiriyle iliskiliyse kullan:
  - ```css
    .box {
      width: 100px;
      height: 100px;
      background-color: yellow;
    }

    .box--small { padding: 20px; }
    .box--medium { padding: 40px; }
    .box--big { padding: 60px; }
    ```
- Birbiriyle ilisliki olan selectorleri indent etmis gibi yazabilirsin:
  - ```css
      .box { }
        .box__small { }
          .box__small--active { }
    ```
- Zor anlasilacak yerleri commentle
  - Buyuk commentlerin oldugu kisimlari `DocBlock` ile yazabilirsin:
    - ```css
      /**
      * Test
      * 
      * 1) Bla bla
      */
      ```
- CSS'in yapisinin HTML yapisiyla ayni nesting oraninda olmasina gerek yok: 
  - ```css
      #app {
        .movie {
          .navbar {

          }
        }
      }
    ```
    yapmak yerine, direkt olarak `navbar` classini secmek hem performans hem de okunabilirlik acisindan (cunku navbar basli basina bir parca) cok daha iyi olur
- HTML'de isimlendirme yaparken class isimlerini birbiriyle iliskilendir
- SCSS'de 3'den fazla nesting yapma
- Class isimlerini daha genel bicimde kullan (`.movies-list-items` yerine `.list` gibi)
- Selectorleri oldukca kisa tutmaya calis
- `!important` rule'unu sadece utility class gibi seyler icin kullan
- Bir selectorun specificitysini arttirman gerekiyor ve location bagimli yapmak istemiyorsan soyle yapabilirsin Gerekmedikce bu yolu kullanma):
  ```css
    .navbar.navbar { } /* Kendisini secip (location bagimsiz) specificitysini 2 ye katliyoruz */
  ```
- ID selectorlerini kullanmaktan kacinmaya calis (specificity'den dolayi). ID selectoru yerine specificity'i dusuk tutmak icin (class selectorle ayni) `[id="navbar"]` (attribute) selectoru kullanilabilir. (Gerekmedikce bu yolu kullanma)
- Componentlerin scalable olmasi icin `absolute` unitlerin yerine `relative` ((em, rem vb.) unitleri kullanmaya calis

<br/>

> ### Bunlari asla unutma: 
  > - your selectors should be as explicit and well reasoned as your reason for wanting to select something.
  > -  it is in our interests not to style things based on where they are, but on what they are
  > - A component shouldn’t have to live in a certain place to look a certain way.
  > - Using a class name to describe content is redundant because content describes itself.
  > - Add any useful or specific meaning via a mechanism that will not inhibit your and your team’s ability to recycle and reuse CSS: `<ul class="tabbed-nav" data-ui-component="Main Nav">`
  > - nesting with preprocessors is often a false economy; as well as making selectors `unnecessarily more specific`, and `creating location dependency`, it also `creates more work for the browser`.
  > - As a rule, if a selector will work without it being nested then do not nest it.

[Kaynak](https://cssguidelin.es/)

Bunlari ogrenmeden once yapmis oldugum [kucuk bir projeyi](https://github.com/umutbozdag/linkedin-redesign/blob/master/main.scss) inceleyip su hatalari yaptigimi farkettim:

- Cok specificity ve fazla gereksiz nesting var. ([CSS Outputuna](https://github.com/umutbozdag/linkedin-redesign/blob/master/main.css) bakarsaniz bu sorun daha kolay anlasiliyor)
- Bazi yerlerde class isimleri kendini ifade edemiyor. Bu yuzden anlamli olmalari icin parent elementine baglilar. (navbar altindaki left, mid, right gibi)
- Bitmis olan kisimlardan sonra bosluk cok az yerde kullanilmis.
- Reusable olabilecek seyler `variable`, `map` veya `list`'e cevrilip kullanilabilirmis. (color, background-color, margin, padding vb.)
- Hic mixin yazilmamis.
- HTML tarafinda BEM kullanilabilirmis.
- Cok fazla specificity olmasaydi belki de `@media` querylerinin icinde `!important` kullanilmasina gerek kalinmayacakti.
- Hic aciklayici comment yazilmamis.