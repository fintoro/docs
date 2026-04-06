---
title: "Začíname"
---

Táto príručka slúži na prvé pripojenie k Fintoro API. Cieľom je overiť, že máte správny token a viete sa zorientovať v ďalších sekciách dokumentácie bez toho, aby sa tu znovu opakovali všetky prevádzkové pravidlá.
## 1. Vytvorte token alebo sandbox firmu

Vo Fintoro otvorte sekciu integrácií pre Fintoro API. Práve tam spravujete tokeny aj sandboxové firmy.

- V produkčnej firme si vytvoríte tokeny pre reálne integračné volania.
- Pod produkčnou firmou si môžete vytvoriť viacero sandboxových firiem pre izolované testovanie.
- Každá firma má vlastné tokeny.

Ak testujete v sandboxe, nepoužívate iný host. Stále voláte `https://app.fintoro.sk/api/public/v1`, ale s tokenom sandboxovej firmy.

## 2. Urobte prvý request

Na prvé overenie odporúčame jednoduchý číselníkový endpoint iba na čítanie:

```bash
curl --request GET \
  --url https://app.fintoro.sk/api/public/v1/countries \
  --header 'Authorization: Bearer <company-token>' \
  --header 'Accept: application/json'
```

Ak je token v poriadku, dostanete `200 OK` a zoznam krajín v `data`.

Ak request vráti `200 OK`, máte overený host aj token.

## 3. Pripravte sa na zápisové scenáre

1. Vytvorte token s najnižším potrebným rozsahom oprávnení.
2. Načítajte si lookupy a referenčné tabuľky, ktoré budete potrebovať pri skladaní request payloadov.
3. Overte read scenáre na reálnej alebo sandboxovej firme.
4. Ak potrebujete reagovať na zmeny priebežne, implementujte [Webhooky](/webhooks). Periodické dotazovanie používajte nanajvýš na prvotný import alebo kontrolné dorovnanie.
5. Pred prvým zápisovým scenárom si prejdite [Konvencie API](/conventions) a [Chyby, rate limiting a retry správanie](/errors-and-idempotency).

## 4. Skontrolujte základné rozhodnutia

- rozhodnite, či integrácia patrí do produkčnej alebo sandboxovej firmy,
- nastavte si naming tokenov podľa systému alebo partnera,
- ak budete prijímať webhooky, pripravte si prijímací endpoint, deduplikáciu a bezpečné uloženie secretu,
- overte si, ktoré lookupy si potrebujete prednačítať do cache alebo lokálneho číselníka,
- prejdite si relevantné tagy v API referencii ešte pred prvým zápisovým scenárom.

## Ďalšie čítanie

- [Autentifikácia](/authentication)
- [Testovanie v sandboxe](/sandbox-testing)
- [Webhooky](/webhooks)
- [Konvencie API](/conventions)
- [Chyby, rate limiting a retry správanie](/errors-and-idempotency)
- [API referencia](/api-reference)
