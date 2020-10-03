# Selectorler nasil calisir?

Browser herhangi bir CSS dosyasi ile karsilastiginda bu CSS dosyasinin icindeki texti CSS Object Model'e parse eder. (tree yapisina donusturur)

Bu parse edilen tree yapisinda eger tek bir selector bulunuyorsa bu selector direkt olarak HTML'deki en tepede olan node'a baglanir. CSS parse edilirken stillerin dogru HTML node'una denk gelmesi istenildigi icin coklu olan selectorler parser tarafindan sagdan sola dogru parse edilir. Bunun nedenini [buradan](performans.md) okuyabilirsiniz.

Ornegin:

```css
button {
  color: blue;
}
```

Yukaridaki ornekteki selector direkt olarak treedeki en tepedeki node'a baglanir

```css
button a {
  color: red;
}
```

Yukaridaki ornekte ise parser sagdan sola dogru parse eder ve sirasiyla:
1- Butun `a` elementlerini bul
2- Bu `a` elementlerinin `button` elementinin altinda olup olmadigini kontrol et adimlarini izler. [Performans acisindan](performans.md) 2. ornekteki selectorun tamamini `.button-link` olarak degistirebiliriz.