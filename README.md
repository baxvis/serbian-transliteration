# Serbian Transliteration - Biblioteka za transkripciju srpskog pisma

[![npm version](https://img.shields.io/npm/v/@baxvis/serbian-transliteration.svg)](https://www.npmjs.com/package/@baxvis/serbian-transliteration)
[![GitHub](https://img.shields.io/badge/github-baxvis/serbian--transliteration-blue?logo=github)](https://github.com/baxvis/serbian-transliteration)

`serbian-transliteration` je lagana i brza JavaScript/TypeScript biblioteka za transkripciju srpskog teksta između ćirilice i latinice. Omogućava jednostavno konvertovanje rečenica ili reči, pogodna je za Node.js i browser okruženja, i ne zahteva dodatne biblioteke.

## Sadržaj
- [Karakteristike](#karakteristike)
- [Instalacija](#instalacija)
- [Primer korišćenja](#primer-korišćenja)
- [API](#api)
- [Dokumentacija](#dokumentacija)
  - [ToCyrillicOptions](#️-tocyrillicoptions)
- [Autor](#autor)
- [Doprinos](#doprinos)
- [Licenca](#licenca)

---

## Karakteristike

- 🔄 Brzo i precizno konvertovanje između ćirilice i latinice
- 📝 Podržava kompletnu srpsku abecedu i azbuku, uključujući digrafe (lj, nj, dž, đ, č, ć, š, ž)
- 📦 Laka za korišćenje - bez spoljnjih biblioteka
- 🌐 Radi u Node.js i browseru
- 🛠️ Jednostavan API za integraciju u druge projekte

---

## Instalacija

Instalirajte paket putem npm-a:
```bash
npm install @baxvis/serbian-transliteration
```

---

## Primer korišćenja

```js
import SerbianTransliteration from '@baxvis/serbian-transliteration'; // ESM syntax
// const SerbianTransliteration = require('@baxvis/serbian-transliteration'); // CommonJS syntax

let result;

// Osnovna transkripcija
result = SerbianTransliteration.toLatin('Љубав и пријатељство');
console.log(result); // Ljubav i prijateljstvo

result = SerbianTransliteration.toCyrillic('Srećno, želim ti lep dan!');
console.log(result); // Срећно, желим ти леп дан!

// Automatsko prepoznavanje pisma i transkripcija
result = SerbianTransliteration.autoTransliterate('Добар dan!');
console.log(result); // Dobar dan!

result = SerbianTransliteration.autoTransliterate('Dobar dan, dobri {ljudi}!', {
    skip: {
        words: ['dobri'],
        markers: ['{', '}']
    }
});
console.log(result); // Добар дан, dobri ljudi!

// Prepoznavanje pisma
const isCyrillic = SerbianTransliteration.isCyrillic('Само ћирилица');
console.log(isCyrillic); // true

const isLatin = SerbianTransliteration.isLatin('Samo latinica');
console.log(isLatin); // true

// Korišćenje opcije skip.words (reči koje se ne transkriptuju)
result = SerbianTransliteration.toCyrillic(
    'Poseti sajt Wikipedia i pročitaj članak.',
    { skip: { words: ['Wikipedia', 'članak'] } }
);
console.log(result); // Посети сајт Wikipedia и прочитај članak.

// Korišćenje opcije skip.markers (delovi koje treba preskočiti)
result = SerbianTransliteration.toCyrillic(
    'Ovo je <skip>some code</skip> koje ne treba transkriptovati.',
    { skip: { markers: ['<skip>', '</skip>'] } }
);
console.log(result); // Ово је some code које не треба транскриптовати.

// Kombinovanje skip.words i skip.markers
result = SerbianTransliteration.toCyrillic(
    'Voli <skip>JavaScript</skip> i koristi StackOverflow svakog dana.',
    { skip: { words: ['StackOverflow'], markers: ['<skip>', '</skip>'] } }
);
console.log(result); // Воли JavaScript и користи StackOverflow сваког дана.

// Automatsko preskakanje reči koje sadrže q, w, x, y
result = SerbianTransliteration.toCyrillic('Ovo je query sa xray i Wendy.');
console.log(result); // Ово је query са xray и Wendy.

// Bacanje greške kad su marker tagovi isti
try {
    result = SerbianTransliteration.toCyrillic(
        'Ovaj <tag>deo</tag> se ne menja',
        { skip: { markers: ['<tag>', '<tag>'] } }
    );
} catch (err) {
    console.error(err); // The opening and closing markers cannot be the same.
}
```

---

## API
| Funkcija                   | Opis                                                           |
|----------------------------|----------------------------------------------------------------|
| `toLatin(text)`            | Pretvara srpski tekst sa ćirilice na latinicu                  |
| `toCyrillic(text, options?)` | Pretvara srpski tekst sa latinice na ćirilicu, uz opcione skip parametre |
| `isCyrillic(text)`         | Vraća `true` ako je većina teksta na ćirilici                  |
| `isLatin(text)`            | Vraća `true` ako je većina teksta na latinici                  |
| `autoTransliterate(text, options?)`  | Automatski detektuje pismo i prevodi tekst u suprotno pismo, uz opcione skip parametre za ćirilicu    |
---

## Dokumentacija

- **toLatin(text: string): string**
  Pretvara srpski tekst iz ćirilice u latinicu.

- **toCyrillic(text: string, options?: ToCyrillicOptions): string**
  Pretvara srpski tekst iz latinice u ćirilicu. Podržava opcije za preskakanje reči i regiona.

- **isCyrillic(text: string): boolean**
  Vraća `true` ako je većina teksta na ćirilici, u suprotnom `false`.

- **isLatin(text: string): boolean**
  Vraća `true` ako je većina teksta na latinici, u suprotnom `false`.

- **autoTransliterate(text: string, options?: ToCyrillicOptions): string**
  Automatski detektuje pismo i prevodi tekst u suprotno pismo (ćirilica ↔ latinica). Podržava opcije za preskakanje reči i regiona za ćirilicu.

### ⚙️ ToCyrillicOptions

Opcioni objekat za funkcije `toCyrillic` i `autoTransliterate` koji omogućava precizno kontrolisanje šta će biti preskočeno tokom transkripcije.

```typescript
/**
 * Opcije za preskakanje reči ili regiona prilikom transkripcije u ćirilicu.
 */
export type ToCyrillicSkipOptions = {
    /** Da li je razlikovanje velikih/malih slova bitno pri poređenju reči i markera */
    caseSensitive?: boolean;
    /** Niz reči koje se neće transkriptovati */
    words?: string[];
    /** Par stringova (otvarajući i zatvarajući marker) koji označavaju deo teksta koji se preskače */
    markers?: [string, string];
};

/**
 * Glavni objekat sa opcijama za ćiriličnu transkripciju.
 */
export type ToCyrillicOptions = {
    /** Opcije za preskakanje reči ili regiona */
    skip?: ToCyrillicSkipOptions;
};
```

**Primer:**
```js
SerbianTransliteration.toCyrillic(
  'Neki <skip>deo</skip> se ne menja.',
  { skip: { words: ['Wikipedia'], markers: ['<skip>', '</skip>'], caseSensitive: false } }
);
```

**Objašnjenje polja:**
- `skip.words`: Reči (stringovi) koje se neće transkriptovati (npr. imena brendova).
- `skip.markers`: Par stringova koji označavaju deo teksta koji treba preskočiti (npr. kod, oznake, tagovi - [`'<skip>'`, `'</skip>'`] podrazumevano).
- `skip.caseSensitive`: Da li poređenje reči/markera razlikuje velika/mala slova (`true` podrazumevano).

---

## Autor

[@baxvis (Jovan Bogovac)](https://github.com/baxvis)

---

## Doprinos

Svi predlozi i poboljšanja su dobrodošli!  
Otvorite pull request ili prijavite problem putem GitHub Issues.

---

## Licenca

Ovaj projekat je licenciran pod MIT licencom.  
Pogledajte `LICENSE` fajl za više detalja.