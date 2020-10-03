# Unitler

CSS'de unitler `absolute` ve `relative` olarak ikiye ayrilir.

## Absolute unit

Absolute unit, parent element veya window boyutuyla direkt olarak ayni olan unitlerdir.

Yani bir elemente absolute unit verdiginizde butun cihazlarda ayni sekilde gozukecek demektir.

### Absolute unitler

- px
- pt
- pc
- cm
- mm
- in

## Relative unit

Relative unit, parentininin veya window'un boyutuna gore degisiklik gosterir.

### Relative unitler

- %
- em (1 em -> 16px)
- rem (root em. 1 rem -> 16px)
- ch
- vh
- vw
- vmin
- vmax

### `em` ile `rem` arasindaki fark

`em` unit'i en yakin parentinin `font-size`'ina gore degisiklik gosterirken, `rem` ise `html`'in font-size'ina gore degisiklik gosterir.