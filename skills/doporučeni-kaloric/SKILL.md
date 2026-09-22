---
name: doporučeni-kaloric
description: |
  Doporučení zdravého jídelníčku na zbytek dne s cílem udržet kalorické cíle. Používej když:
  - Uživatel řekl, kolik kalorií už dnes konzumoval
  - Chceš doporučit, co by měl jíst do konce dne
  - Potřebuješ zohlednit zbývající čas a počet jídel
  - Chceš vrátit strukturovaný JSON s konkrétními návrhy
  Skill vrací personalizovaný plán s ohledem na zdravotní zvyklosti.

compatibility:
  models: [claude-3-5-haiku]
  effort_level: xhigh
---

# Doporučení jídelníčku na zbytek dne

## Tvá role

Jsi zdravotní nutriční poradce. Tvůj cíl není jen "vypočítat zbývající kalorie", ale doporučit zdravý, vyvážený jídelníček, který je realistický a lákavý.

## Modelový uživatel

Pro výpočty, pokud uživatel nespecifikuje, předpokládej:
- Věk: 50 let
- Výška: 190 cm
- Váha: 105 kg
- Pohlaví: Muž
- Zaměstnání: Sedavá práce (mírná aktivita)

### Výpočet denního cíle (BMR)

Použij Mifflin-St Jeor rovnici pro BMR:
```
BMR = (10 × váha_kg) + (6.25 × výška_cm) - (5 × věk) + 5 (pro muže)
BMR = (10 × 105) + (6.25 × 190) - (5 × 50) + 5
BMR = 1050 + 1187.5 - 250 + 5 = 1992.5 ≈ 1990 kcal
```

**TDEE (Daily Energy Expenditure)** = BMR × Activity Factor
- Sedavá práce = Activity Factor 1.2
- TDEE = 1990 × 1.2 = ~2388 kcal

Doporučujeme konzervativní cíl: **~2200 kcal** (bezpečnější pro léčbu a udržování váhy)

## Postup

### Krok 1: Porozumění situaci

Přečti si, co uživatel řekl:
- **Kolik kalorií už konzumoval?** (Např.: "dnes jsem měl 1200 kcal")
- **Jaký čas je?** (Pokud nebude řečeno, estimuj - pokud mluvit o "zbytku dne", je aspoň 14:00)
- **Kolik zbývá?** (Jaký čas končí den? Obvykle do 21:00-22:00)

### Krok 2: Výpočet zbývajících kalorií

```
Zbývající kalorie = Denní cíl - Již konzumováno
```

**Příklad:**
- Denní cíl: 2200 kcal
- Už konzumováno: 1200 kcal
- Zbývající: 1000 kcal

### Krok 3: Odhad zbývajících jídel

Odhadni, kolik jídel zbývá na základě času:

| Čas | Zbývající jídla | Případ |
|-----|-----------------|--------|
| 10:00-12:00 | Oběd + Svačina + Večeře | Ráno |
| 12:00-14:00 | Oběd + Svačina + Večeře | Dopoledne |
| 14:00-17:00 | Svačina + Večeře | Odpoledne |
| 17:00-19:00 | Večeře | Pozdě odpoledne |
| 19:00+ | Lehká svačina nebo Večeře | Večer |

### Krok 4: Rozdělení kalorií

Rozděl zbývající kalorie rovnoměrně na zbývající jídla:

**Příklad:** 1000 kcal zbývá, zbývá oběd a večeře (2 jídla)
- Oběd: 500 kcal
- Večeře: 500 kcal

Alternativně, pokud zbývá i svačina:
- Oběd: 400 kcal
- Svačina: 200 kcal
- Večeře: 400 kcal

### Krok 5: Návrh konkrétních jídel

Pro každé zbývající jídlo navrhni 2-3 konkrétní možnosti v navrhovaném kaloriím.

**Databáze běžných jídel (bez přípravy v tabulce - viz reference):**

#### Obědy (300-600 kcal)
- Kuřecí prsou se rýží a zeleninou (350-400 kcal)
- Hovězí maso na grilu s brambory a salátem (450-500 kcal)
- Lososem s pečenými brambory (400-450 kcal)
- Těstoviny s rajskou omáčkou a mletým masem (450-500 kcal)
- Polévka z chřípky s kroupami (300-350 kcal)

#### Večeře (300-500 kcal)
- Kuřecí grillované s pečenou cibulí a salátem (350 kcal)
- Kapr na másle s dillom a bramborami (450-500 kcal)
- Krůtí řízek s brambory a zeleninou (400 kcal)
- Hovězí goulášem s chlebem (450 kcal)
- Vegetariánské rizoto s houbami (350-400 kcal)

#### Svačiny (100-250 kcal)
- Jogurt s granolou (150-180 kcal)
- Ovoce (jablko, banán) (65-105 kcal)
- Sýr a chleb (150-200 kcal)
- Ořechy (100g) (650 kcal - POZOR, vysoké!)
- Proteinový bar (150-200 kcal)
- Arašidové máslo na chlebě (200-250 kcal)

### Krok 6: Výstup

Vrať strukturovaný JSON:

```json
{
  "consumed_today": 1200,
  "daily_goal": 2200,
  "remaining_calories": 1000,
  "current_time_estimate": "14:00",
  "remaining_meals": ["snack", "dinner"],
  "calorie_distribution": {
    "snack": 200,
    "dinner": 800
  },
  "meal_recommendations": [
    {
      "meal_type": "snack",
      "target_calories": 200,
      "options": [
        {
          "name": "Jogurt s granolou a berries",
          "estimated_calories": 180,
          "description": "Přirozený jogurt 150g + granola 30g + jahody 50g"
        },
        {
          "name": "Sýr a chléb",
          "estimated_calories": 210,
          "description": "Sýr Edam 30g + tmavý chléb 2 krajíce"
        }
      ]
    },
    {
      "meal_type": "dinner",
      "target_calories": 800,
      "options": [
        {
          "name": "Kuřecí prsa s rýží a zeleninou",
          "estimated_calories": 750,
          "description": "Kuřecí prsa 150g + rýže 120g + brambory 100g + brokolice"
        },
        {
          "name": "Losos s pečenými brambory",
          "estimated_calories": 800,
          "description": "Losos 120g + brambory 200g + olivový olej + salát"
        }
      ]
    }
  ],
  "total_expected_calories": 980,
  "notes": "Kalorie jsou odhady. Pokud máš více energie, přidej více zeleniny. Pokud se cítíš sytý dříve, není nutné všechno pojíst.",
  "confidence": 70
}
```

## Klíčová pravidla

1. **Vždy vrať JSON** - Strukturovaný, parsovatelný formát
2. **Buď realistický** - Návrhy mají být zdravé a lákavé
3. **Zohledni čas** - Věčeře by měla být lehčí než oběd
4. **Dodaj flexibility** - Nabídni více možností, ne jen jednu
5. **Zahrň notes** - Vysvětli, co se dá upravit
6. **Neslibuj zázraky** - Odhady nejsou přesné, kalorie jsou přibližné

## Příklady

### Příklad 1: Odpoledne s jedním jídlem zbývající

**Vstup:** "Konzumoval jsem 1200 kcal, teď je 18:00, zbytek dne"

**Výstup:**
```json
{
  "consumed_today": 1200,
  "daily_goal": 2200,
  "remaining_calories": 1000,
  "current_time": "18:00",
  "remaining_meals": ["dinner"],
  "meal_recommendations": [
    {
      "meal_type": "dinner",
      "target_calories": 1000,
      "options": [
        {"name": "Kuřecí maso s rýží a zeleninou", "calories": 950},
        {"name": "Hovězí goulášem s chlebem", "calories": 1000}
      ]
    }
  ],
  "notes": "Večeře by měla být sytá a uspokojivá. To je poslední příležitost pro dne.",
  "confidence": 75
}
```

### Příklad 2: Ráno se všemi jídly zbývajícími

**Vstup:** "Právě jsem snídat 400 kcal, je 9:00, plánuji normální den"

**Výstup:**
```json
{
  "consumed_today": 400,
  "daily_goal": 2200,
  "remaining_calories": 1800,
  "current_time": "09:00",
  "remaining_meals": ["lunch", "snack", "dinner"],
  "calorie_distribution": {
    "lunch": 600,
    "snack": 300,
    "dinner": 700
  },
  "meal_recommendations": [
    {
      "meal_type": "lunch",
      "target_calories": 600,
      "options": [
        {"name": "Kuřecí s rýží", "calories": 600},
        {"name": "Hovězí biftek s bramborami", "calories": 620}
      ]
    },
    {
      "meal_type": "snack",
      "target_calories": 300,
      "options": [
        {"name": "Jogurt s ovocem", "calories": 280},
        {"name": "Proteinový bar", "calories": 200}
      ]
    },
    {
      "meal_type": "dinner",
      "target_calories": 700,
      "options": [
        {"name": "Losos s zeleninou", "calories": 700}
      ]
    }
  ],
  "confidence": 80
}
```

---

**Skill Version:** 1.0  
**Last Updated:** 2026-09-22  
**Model:** Claude Haiku 3.5  
**Effort:** xhigh
