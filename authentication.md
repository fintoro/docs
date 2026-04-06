---
title: "Autentifikácia"
---

# Autentifikácia

Fintoro Public API používa bearer tokeny. Token je vždy vydaný pre konkrétnu firmu.

## Hlavička Authorization

Každý request posielajte s hlavičkou `Authorization: Bearer <company-token>`.

```bash
curl --request GET \
  --url https://app.fintoro.sk/api/public/v1/clients \
  --header 'Authorization: Bearer <company-token>' \
  --header 'Accept-Language: en' \
  --header 'Accept: application/json'
```

`Accept-Language` je voliteľný header pre ktorýkoľvek Public API request. Lokalizuje systémové názvy vo fixných lookupoch aj v rovnakých vnorených lookup objektoch v response-och a ovplyvňuje aj validačné chyby. Podporované tagy a fallback pravidlá nájdete v [Konvenciách API](./conventions.md). User-generated dáta tým nemeníte.

## Model oprávnení

- `read` je určený pre synchronizácie, reporting, BI a ostatné read-only scenáre.
- `write` povoľuje čítanie aj zápis vrátane create, update a delete operácií tam, kde to endpoint podporuje.
- `write` je potrebný aj na správu odberov webhookov, teda na create, update, delete a rotate-secret operácie.

Odporúčame používať najnižší rozsah oprávnení, ktorý integrácia potrebuje.

## Firemný kontext tokenu

- Token je vždy vydaný pre konkrétnu firmu.
- Rovnakú základnú URL používajú produkčné aj sandboxové firmy.
- Ak chcete testovať izolovane, použite token sandboxovej firmy.
- Pod jednou produkčnou firmou môžete mať viacero sandboxových firiem a každá z nich má vlastné tokeny.

## Praktické bezpečnostné pravidlá

- Token si po vygenerovaní bezpečne uložte. Hodnota v otvorenej podobe sa zobrazí len pri vytvorení.
- Webhook secret nie je bearer token. Ukladajte ho oddelene a nikdy ho nepoužívajte na autorizáciu Public API requestov.
- Nepoužívajte jeden token pre viac rôznych integrácií, ak ich viete oddeliť.
- Tokeny pomenujte podľa systému alebo partnera, aby sa dali dobre auditovať.
- Nepoužívané alebo kompromitované tokeny revokujte. Revokovaný token okamžite prestane fungovať v Public API a ďalšie requesty skončia na `401 Unauthenticated.`, ale token aj jeho auditná stopa zostanú vo Fintoro dohľadateľné.

## Kde tokeny spravovať

Tokeny spravujete vo Fintoro v sekcii `Integrácie > Public API`. Na rovnakom mieste v produkčnej firme vytvoríte aj sandboxové firmy a následne pre ne spravujete tokeny sandboxovej firmy.
