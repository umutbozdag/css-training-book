# Combinatorler

Combinatorler selectorler arasindaki iliskiyi ifade etmemize yarar.

## Descendant selector

Bir elementin altindaki belirttigimiz butun elementleri secmemize yarar.

```css
  main p { } /* main elementinin altindaki butun p elemnetlerini sec */
```
## Child selector

Belirtilen bir elementin butun child elementlerini secmemize yarar.

```css
  main > p { } /* sadece main elementinin direkt child'i olan p elementlerini sec */
```

## Adjacent Sibling Selector

Belirtilen bir elementin sadece ilk sibling elementini (kardes) secmemize yarar.

```css
  main + p { } /* main elementinin sadece ilk sibling elementini (ayni parent altinda) sec */
```

## General Sibling Selector

Belirtilen bir elementin butun sibling elementlerini secmemize yarar

```css
  main ~ p { } /* main elementinin sibling'i olan butun p elementlerini sec */
```