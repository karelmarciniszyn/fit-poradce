# Skill: Odhad kalorií

## Účel

Skill `odhad-kaloric` odhaduje kalorickou hodnotu a makronutrienty (proteiny, tuky, sacharidy) z textového popisu potraviny nebo nápoje.

## Kdy jej používat

Zavoláš skill `odhad-kaloric` když:
- Uživatel řekl co právě konzumoval (např. "jedl jsem vejce s chlebem")
- Potřebuješ odhad kalorií pro záznam do aplikace
- Chceš strukturovaný výstup pro zpracování

## Vstup

Skill očekává textový popis potraviny ve volné formě:
- "Jedl jsem talíř těstovin s rajskou omáčkou"
- "Kuřecí prsa 150g se zeleninou"
- "Dvě jablka a sklenice mléka"

## Výstup

Skill vrací JSON v tomto formátu:
```json
{
  "food_description": "...",
  "parsed_items": [...],
  "total_calories": 210,
  "total_protein": 5.5,
  "total_fat": 6.5,
  "total_carbs": 32.5,
  "confidence": 65,
  "confidence_reason": "...",
  "notes": "..."
}
```

## Přesnost

- **Vysoká přesnost:** Když uživatel udá jasné množství (100g, 2 vejce, atd.)
- **Střední přesnost:** Když řekne "talíř" nebo "sklenice" (bez přesných gramů)
- **Nízká přesnost:** Velmi vágní popis typu "něco mě nasytilo"

Skill vždy indikuje `confidence` skóre, které ukazuje, jak si je jistý.

## Limitace

1. **Odhady nejsou přesné** - Bez přesného vážení jsou to jen přibližné hodnoty
2. **Úprava při vaření** - Smažení vs. vařování mění kalorie
3. **Chybí mikronutrienty** - Fokus je jen na kaloriích a makro
4. **Databáze je omezená** - Pokud uživatel zmíní neznámou potravinu, odhad je méně přesný

## Příklady

### Jednoduchý odhad
```
Vstup: "Jedl jsem banán"
Výstup: ~105 kcal, 1.3g protein, 0.3g tuk, 27g sacharidy
Confidence: 90%
```

### Složitější odhad
```
Vstup: "Kuřecí prsa 150g se 200g brambor a zeleninou"
Výstup: ~320 kcal (bez tuku na vaření)
Confidence: 70% (nevíme přesný obsah tuku)
```

## Integrace s aplikací

Skill je volán přes Claude API. Web interface pošle:
```javascript
const response = await fetch('...', {
  prompt: `Odhadni kalorie: ${userInput}`
})
```

Skill vrátí strukturovaný JSON.

## Testování

Testovacích případů je 3:
1. "Jedl jsem talíř těstovin s rajskou omáčkou" → ~200-215 kcal
2. "Dvě jablka a sklenice mléka" → ~170 kcal
3. "Kuřecí prsa (150g) se zeleninou" → ~250-350 kcal (záleží na tuky)

---

**Verze:** 1.0  
**Model:** Claude Haiku 3.5 (effort: xhigh)  
**Poslední update:** 2026-09-22
