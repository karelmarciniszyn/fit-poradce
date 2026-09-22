# Fit Poradce

Aplikace pro odhad kalorií a doporučení zdravého jídelníčku s pomocí umělé inteligence.

## O aplikaci

**Fit Poradce** je nástroj, který vám pomůže:
- Odhadnout kalorické hodnoty vašich jídel a nápojů na základě popisu
- Dostat doporučení na zbytek dne pro udržení vaších kalorických cílů
- Dosáhnout zdravého stravovacího režimu

## Modelový uživatel

Aplikace je optimalizována pro typického sedavého muže:
- Věk: 50 let
- Výška: 190 cm
- Váha: 105 kg
- Zaměstnání: Sedavá práce

## Jak to funguje

Aplikace používá dva Claude skills:

1. **odhad-kaloric** - Odhaduje kalorickou hodnotu potraviny z jejího popisu
2. **doporučeni-kaloric** - Doporučuje plán na zbytek dne

Veškerá logika je řešena AI modelem, žádné hardcoded tabulky.

## Webové rozhraní

Aplikace je dostupná na webu jako GitHub Pages. Obsahuje:
- Formulář pro odhad kalorií
- Formulář pro doporučení jídelníčku
- Informace o aplikaci

## Technologie

- Claude Haiku 3.5 (effort: xhigh)
- Claude API
- HTML + CSS + JavaScript (vanilla)

## Jak spustit lokálně

1. Klonuj repository
2. Otevři `index.html` v prohlížeči
3. Zadej svůj Claude API key (získáš na https://console.anthropic.com)

## GitHub Pages

Aplikace je automaticky publikována na GitHub Pages.

## Kontakt

Odesláno na: petr.rozkoschny@ictpro.cz

## Licence

MIT
