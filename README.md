# Airbnb CSS / Sass stil rehberi

*CSS ve Sass'a daha anlamli yaklasim*

## Icindekiler

1. [Terminoloji](#terminology)
    - [Kural Tanimlamari](#rule-declaration)
    - [Seciciler](#selectors)
    - [Ozellikler](#properties)
1. [CSS](#css)
    - [Formatlama](#formatting)
    - [Yorumlar](#comments)
    - [OOCSS ve BEM](#oocss-and-bem)
    - [ID Secicileri](#id-selectors)
    - [JavaScript kancalari](#javascript-hooks)
    - [Cizgiler](#border)
1. [Sass](#sass)
    - [Yazim bicimi](#syntax)
    - [Siralama](#ordering-of-property-declarations)
    - [Degiskenler](#variables)
    - [Mixin'ler](#mixins)
    - [Extend direktifi](#extend-directive)
    - [Alt seciciler](#nested-selectors)
1. [Ceviri](#translation)
1. [Lisans](#license)

## Terminoloji

### Kural tanimlamalari

“Kural tanimi” dedigimiz, bir stil sinifina verdigimiz isim ve altindaki css ozelligi tanimlamaridir. Ornek: 

```css
.liste {
  font-size: 18px;
  line-height: 1.2;
}
```

### Seciciler

Bir kural taniminda, seciciler, html'de DOM agacinda hangi elemanlarin bu siniftaki stillemeye sahip olacagini belirlememizi saglar. Seciciler html elemanlarina, o elemanin sinif degerine, ID tanimina veya herhangi bir html etiket alt tanimlamasina uygun olabilir. Ornegin: 

```css
.benim-sinifim {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Ozellikler

Son olarak, stillemede, ozellikler dedigimiz seyler, seciciyle tanimlanmis elemanlardaki stil tanimlamalaridir. Anahtar ve deger ikilileri olarak yazilirlar ve bir sinif birden fazla stil tanimlamasina sahip olabilir. Ozellik tanimlamalari asagidaki ornekte gorulerbilir:

```css
/* herhangi bir secici */ {
  background: #f1f1f1;
  color: #333;
}
```

**[⬆ sayfa basina don](#table-of-contents)**

## CSS

### Formatlama

* Bosluk karakteri (2 karakter) kullanarak indent yapin
* Buyuklu kucuklu degisken isimleri yerine tire isareti kullanmayi tercih edin
  - BEM ([OOCSS ve BEM](#oocss-and-bem)) teknigini kullaniyorsaniz, alt cizgi ve BuyukHarfBaslangic kullanabilirsiniz.
* ID secicilerini kullanmayin
* Birden fazla secici kullanarak bir CSS sinifi tanimliyorsaniz, her seciciyi ayri satirda yazmaya ozen gosterin.
* Acilis parantezinden `{` once bosluk birakin.
* Ozellikleri belirtirken, `:` karakterinden sonra bosluk birakin. Oncesinde bosluk birakmayin.
* Kapaniz parantezinden `{` sonra bosluk birakin ve ayri satirda yazin.
* CSS sinif tanimlamalari arasinda bosluk satir birakin.

**Yanlis**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Dogru**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Yorumlar

* Tek satirlik yorum tanimlamalarini kullanmaya calisin (Sass dunyasinda `//` karakterleri kullaniliyor).
* Yorumlari kendi satirlarinda tutun. Varolan bir kodun sonuna yorum eklemekten kacinin.
* Sadece koddan okunarak anlasilmayacak detaylari yorumlayin:
  - z-index kullanimlari
  - Tarayici specifik hack'leri

### OOCSS ve BEM

OOCSS ve/veya BEM kombinasyonunu kullanmanizi, asagidaki ajantalardan dolayi tavsiye ediyoruz:

  * Acik, HTML ve CSS arasinda anlasilir bir iliski saglamak icin
  * Tekrar kullanilabilir siniflar tanimlayabilmek icin
  * Daha az ic ice sinif tanimlama sagladigi icin
  * Daha olceklendirilebilir (giderek buyuyebilecek) CSS dokumanlari hazirlamamizi sagladigi icin

**OOCSS**, ya da acikca “Object Oriented CSS”, bir cesit CSS yazma yaklasimi olup, CSS dokumanlariniza, nesne odakli dusunmenizi saglayacak boylece, tekrar kullanilabilir ve bagimsiz siniflar uretmenizi saglar.

  * Nicole Sullivan'in [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine'in [OOCSS'e Giris](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, veya acikca “Block-Element-Modifier”, HTML ve CSS icin bir _isimlendirme modelidir_. Yandex takimi tarafindan ortaya artilmisve cok buyuk kod ve stil dokumanlarini daha anlasilir yonetebilmek icin olusturulmus bir yaklasimdir.

  * CSS Trick'in [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts'in [BEM'e Giris](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

BEM'i BuyukHarfDegiskenIsimlendirmesi seklinde kullanmanizi tavsiye ediyoruz. OZellikle React gibi kutuphanelerle de birebir uyum gosterecektir.

**Ornek**

```jsx
// ListingCard.jsx
function ListingCard() {
  return (
    <article class="ListingCard ListingCard--featured">

      <h1 class="ListingCard__title">Terasli, 2 oda 1 salon</h1>

      <div class="ListingCard__content">
        <p>Vestibulum id ligula porta felis euismod semper.</p>
      </div>

    </article>
  );
}
```

```css
/* ListingCard.css */
.ListingCard { }
.ListingCard--featured { }
.ListingCard__title { }
.ListingCard__content { }
```

  * `.ListingCard` en tepedeki nesneyi temsil ediyor
  * `.ListingCard__title` nesnesi, `.ListingCard`in bir alt nesnesi olup tum blogun bir parcasidir that helps compose the block as a whole.
  * `.ListingCard--featured` ise, `.ListingCard` ana nesnesinin bir durumunu (state) temsil ediyor.

### ID secicileri

CSS'de her ne kadar ID'ler ile secim yapabilsek de, genel olarak negatif bir gelistirme modelidir (anti-pattern). ID secicileri, CSS'in hiyerarsik secicileri karsisinda gereksiz bir dogrudan secim ([specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)) belirtir ve tekrar kullanilabilirligi dusuk CSS siniflari tanimlamaniza yol acar.

Bu konuda daha detayli okuma materyali icin [CSS Wizardry'in makalesini](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) okumanizi tavsiye ederiz.

### JavaScript kancalari (hooks)

Ayni sinif isimlerini hem CSS hem JavaScript icin kullanmayin. Bu iki kullanimi karistirmak gelistirici icin takip etmesi zor ve kafa karisikligi olusturabilecek kod uretmenize yol acar.

JavaScript-spesifik siniflari `.js-` on ekiyle isimlendirmenizi tavsiye ederiz:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Kenar cizgileri

Kenar cizgisi olmayan bir nesneyi tanimlarken, `none` yerine `0` kullanin.

**Yanlis**

```css
.foo {
  border: none;
}
```

**Dogru**

```css
.foo {
  border: 0;
}
```
**[⬆ sayfa basina don](#table-of-contents)**

## Sass

### Yazim Stili

* `.scss` yazim bicimini kullanin. Orijinal `.sass` yazim bicimini kullanmayin.
* Genel CSS'inizi ve `@include` kullaniminizi mantiksal sekilde siralayin (daha detayli aciklama asagida)

### Ozelliklik tanimlamalarinin siralamasi

Asagidaki siralamayi izleyin:

1. Ozellik tanimlamalari

    Ilk basta, `@include` disindaki standard ozellik tanimlamalarinizi belirtin.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` kullanimi

    `@include` tanimlarinizi standard CSS ozelliklerinden hemen sonra gruplayin, boylece daha okunabilir olacaktir tum sinif taniminiz.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Ic ice seciciler

    Ic ice seciciler bir sinif tanimi icinde en son sirada yazilir. Ic ice secicilerden sonra veya aralarinda baska tanimlamalar girmez. Yukaridaki siralamayi ic ice sinif taniminizda da kullanmaya devam edin.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Degiskenler

`kucukluBuyukluDegisken` veya `alltan_cizgili` degisken isimleri yerine duz cizgi ile ayrilmis degisken isimleri tanimlamaya dikkat edin (ornek. `$buyuk-baslik`).

### Mixin'ler

Mixin'ler kodda tekrari ortadan kaldirmaya yardim edip, karmasikligi sadelestirmeye yarayan methoddur. Unutmamak gerek ki eger parametresiz mixin'leri cok kullandigimizda ortaya cikan son derlenmis css boyutu eger paketlenmemizsse (ornegin gzip), buyuk dosya boyutuna ve gereksiz tekrarlara neden olabilir.

### Extend kullanimi

`@extend` kullanimi, ozellikle ic ice kullanimlarda potansiyel olarak tehlikeli sonuclar dogurabilir. Bunun yerine mixin'leri kullanmak, ozellikle de gzip'leme methodu kullanarak, ortaya cikacak buyuk dosya boyutu problemini kolaylikla cozecektir.

### Ic ice seciciler

**Uc derinlikten fazla ic ice sinif tanimlamaktan kacinin!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

Eger ic ice seciciler bu kadar uzun hale gelince, ortaya cikan CSS, asagidakilere neden olacaktir:

* HTML gibi karmasiklasak
* Fazlasiyla spesifik
* Tekrar kullanilamaz hale gelecek


Tekrar: **asla ID secicilerini ic ice yazmayin!**

Eger illa ki ID secicisi kullanacaksaniz (ve kullanmanizdan kacinmanizi tavsiye ediyoruz), bu seciciler asla ic ice yazilmamalidir.

**[⬆ sayfa basina don](#table-of-contents)**

## Ceviriler
  
  Bu stil rehberi, asagidaki diger dillerde de okunabilir:

  - ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Bahasa Indonesia**: [mazipan/css-style-guide](https://github.com/mazipan/css-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [ArvinH/css-style-guide](https://github.com/ArvinH/css-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [Zhangjd/css-style-guide](https://github.com/Zhangjd/css-style-guide)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [mat-u/css-style-guide](https://github.com/mat-u/css-style-guide)
  - ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [nao215/css-style-guide](https://github.com/nao215/css-style-guide)
  - ![ko](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [CodeMakeBros/css-style-guide](https://github.com/CodeMakeBros/css-style-guide)
  - ![PT-BR](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portuguese (Brazil)**: [felipevolpatto/css-style-guide](https://github.com/felipevolpatto/css-style-guide)
  - ![pt-PT](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Portugal.png) **Portuguese (Portugal)**: [SandroMiguel/airbnb-css-style-guide](https://github.com/SandroMiguel/airbnb-css-style-guide)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [Nekorsis/css-style-guide](https://github.com/Nekorsis/css-style-guide)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [ismamz/guia-de-estilo-css](https://github.com/ismamz/guia-de-estilo-css)
  - ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [mfyz/airbnb-css-tr](https://github.com/mfyz/airbnb-css-tr)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [trungk18/css-style-guide](https://github.com/trungk18/css-style-guide)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [antoniofull/linee-guida-css](https://github.com/antoniofull/linee-guida-css)

**[⬆ sayfa basina don](#table-of-contents)**

## Lisans (Orijinal/Ingilizce)

(The MIT License)

Copyright (c) 2015 Airbnb

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ sayfa basina don](#table-of-contents)**
