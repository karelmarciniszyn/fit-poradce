# Skill: Doporučení kalorického jídelníčku

## Účel

Skill `doporučeni-kaloric` doporučuje, co by měl uživatel jíst na zbytek dne, aby dosáhl svého denního cíle kalorií.

## Kdy jej používat

Zavoláš skill `doporučeni-kaloric` když:
- Uživatel řekl, kolik kalorií už dnes konzumoval (např. "dnes jsem měl 1200 kcal")
- Potřebuješ návrh na zbývající jídla
- Chceš personalizovaný plán, nikoli jen seznam potravin

## Vstup

Skill očekává informace:
- Kolik kalorií už konzumoval (nebo co jedor)
- Jaký je aktuální čas (nebo "zbytek dne")
- Případně další kontexty (dietní omezení, preference)

Příklady:
- "Konzumoval jsem 1200 kcal dnes, je 14:00, co si mám dat k večeři?"
- "Snídaně měla 400 kcal, teď je 9:00, plánuji normální den"
- "Měl jsem lehký oběd (500 kcal), zbývá mi ještě 1700 kcal na zbytek dne"

## Výstup

Skill vrací JSON s tímto formátem:
```json
{
  "consumed_today": 1200,
  "daily_goal": 2200,
  "remaining_calories": 1000,
  "remaining_meals": ["dinner"],
  "meal_recommendations": [
    {
      "meal_type": "dinner",
      "target_calories": 1000,
      "options": [...]
    }
  ],
  "notes": "...",
  "confidence": 75
}
```

## Denní cíl kalorií

Pro modelového uživatele (muž, 50 let, 105 kg, sedavá práce) je denní cíl:
- **BMR (Basal Metabolic Rate):** ~1990 kcal
- **TDEE (Daily Energy Expenditure):** ~2388 kcal
- **Doporučený cíl:** ~2200 kcal (konzervativní)

Skill si sám počítá cíl podle BMR, ale lze jej také zadadit.

## Personalizace

Skill by měl zohledňovat:
- **Čas:** Běžný režim jídel je snídaně (7-9), oběd (12-14), svačina (16-17), večeře (19-21)
- **Zbývající čas:** Pokud je už 19:00, zbývá jen večeře
- **Preference:** Lze specifikovat dietní omezení (bez masa, bez lepku, atd.)

## Příklady doporučení

### Obědy (500-650 kcal)
- Kuřecí prsa s rýží a zeleninou (550-600 kcal)
- Hovězí maso na grilu (600-650 kcal)
- Losos s brambory (550 kcal)

### Večeře (400-700 kcal)
- Kuřecí grillované (400-450 kcal)
- Krůtí řízek s brambory (450-500 kcal)
- Hovězí goulášem (600-650 kcal)

### Svačiny (150-300 kcal)
- Jogurt s ovocem (150-200 kcal)
- Sýr a chléb (200-250 kcal)
- Proteinový bar (200-250 kcal)

## Limitace

1. **Odhady kalorií** - Všechny kalorie jsou přibližné
2. **Individuální variace** - Každý člověk má jinou spotřebu
3. **Bez medicínské rady** - Skill není náhrada za lékaře
4. **Není nutné dodržovat přesně** - Pokud je člověk sytý, nemusí vše pojíst

## Integrace s aplikací

Skill je volán přes Claude API. Web interface pošle:
```javascript
const response = await fetch('...', {
  prompt: `Konzumoval jsem ${calorieInput} kcal, je ${timeEstimate}, co si mám dát?`
})
```

Skill vrátí strukturovaný JSON s doporučeními.

## Testování

Testovacích případů je 3:
1. Konzumoval 1200 kcal do 14:00 → Zbývá 1000 kcal (oběd + svačina + večeře)
2. Konzumoval 800 kcal do 10:00 → Zbývá 1400 kcal (snídaně + oběd + svačina + večeře)
3. Konzumoval 2000 kcal do 18:00 → Zbývá 200 kcal (lehká svačina)

---

**Verze:** 1.0  
**Model:** Claude Haiku 3.5 (effort: xhigh)  
**Poslední update:** 2026-09-22
