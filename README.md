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

Projekt má dvě části, které řeší totéž dvěma různými způsoby.

**Claude skills** (složka `skills/`) — odhad dělá model, takže zvládne volný text
i potraviny, které nikdo předem nevyjmenoval:

1. **odhad-kaloric** — odhadne kalorickou hodnotu potraviny z jejího popisu
2. **doporučeni-kaloric** — doporučí plán na zbytek dne

**Webová stránka** (`index.html`) — počítá si vlastním JavaScriptem nad databází
zhruba padesáti běžných potravin. Stránka skills nevolá a na nic se nepřipojuje.

Kompromis je vědomý: stránka je vstupní branou pro reklamu, takže musí fungovat
okamžitě a bez přihlašování. Cenou je, že rozpozná jen potraviny ze své databáze —
hotová jídla jako svíčková nebo sushi v ní nejsou a nikdy všechna nebudou.

Když potravinu nezná, stránka nevrací nulu, ale přizná, že odhad nemá, a nabídne
tlačítko, které zkopíruje připravený prompt a otevře Claude. Tam už jídlo odhadne
skill `odhad-kaloric`, který si s volným textem poradí. Žádný API klíč to nevyžaduje.

## Webové rozhraní

Aplikace je dostupná na webu jako GitHub Pages. Obsahuje:
- Nastavitelný profil (věk, výška, váha, pohlaví) s živým přepočtem denního cíle
- Formulář pro odhad kalorií
- Formulář pro doporučení jídelníčku

Denní cíl se počítá rovnicí Mifflin-St Jeor s faktorem 1,2 pro sedavou práci.
Profil se ukládá do prohlížeče; když je úložiště zakázané, aplikace funguje dál,
jen se nastavení nepřenese do dalšího spuštění.

## Technologie

- Skills: Claude Haiku (effort: xhigh)
- Web: HTML + CSS + JavaScript (vanilla), bez závislostí a bez buildu

## Jak spustit lokálně

1. Klonuj repository
2. Otevři `index.html` v prohlížeči

Žádný API klíč ani připojení k internetu není potřeba — stránka počítá všechno
lokálně v prohlížeči.

## GitHub Pages

Aplikace je automaticky publikována na GitHub Pages.

## Kontakt

Odesláno na: petr.rozkoschny@ictpro.cz

## Licence

MIT
