---
title: "Fintoro Public API"
sidebarTitle: "Úvod"
---

# Fintoro Public API

Fintoro Public API je REST API na integráciu Fintoro fakturácie, CRM a skladového systému do systémov tretích strán. Je určené pre partnerov, interné integračné tímy aj zákaznícke implementácie, ktoré potrebujú spoľahlivo čítať a zapisovať údaje z Fintoro a zároveň reagovať na zmeny cez odchádzajúce webhooky.

Použite ho na prácu s klientmi, dodávateľmi, CRM, skladmi, dokladmi, odbermi webhookov a číselníkovými údajmi nad jednou produkčnou alebo sandboxovou firmou.

Produkčné aj sandboxové firmy používajú rovnakú základnú URL `https://app.fintoro.sk/api/public/v1`. Firemný kontext neurčuje iný host ani iná cesta, ale bearer token firmy, pre ktorú bol token vydaný.

## Čo dokumentácia obsahuje

- úvodné príručky pre prvé nasadenie a prevádzku integrácie,
- príručku k webhookom, podpisovaniu, payloadu doručenia a katalógu eventov,
- OpenAPI referenciu pre presný request a response kontrakt,
- referenčné tabuľky pre fixné číselníky so stabilnými ID,
- vysvetlenie autentifikácie, sandbox firiem, idempotencie a sledovania požiadaviek.

Ak Vaša integrácia potrebuje reagovať na zmeny priebežne a držať downstream systém zosynchronizovaný s čo najmenším oneskorením, implementujte [Webhooky](./webhooks.md). Na to sú v Public API určené. Čítacie endpointy používajte na prvotný import, doťahovanie detailu a kontrolné dorovnanie, nie ako hlavný mechanizmus detekcie zmien.

## Base URL

```plaintext
https://app.fintoro.sk/api/public/v1
```

## Začíname

Na prvé pripojenie pokračujte do príručky [Začíname](./getting-started.md), kde nájdete vytvorenie tokenu alebo sandboxovej firmy, prvý autentifikovaný request aj odporúčaný ďalší postup pre zápisové scenáre a webhooky.

## Produkčné a sandboxové firmy

- Pod jednou produkčnou firmou si môžete vytvoriť viacero sandboxových firiem.
- Každá sandboxová firma slúži ako izolované prostredie na testovanie API.
- Sandboxová firma má obmedzený prístup k niektorým sekciám Fintoro, ale na testovanie Public API funguje cez vlastné tokeny.
- Pre API requesty zostáva host aj základná cesta rovnaká. Mení sa len token.

## Ďalšie čítanie

- [Začíname](./getting-started.md)
- [Autentifikácia](./authentication.md)
- [Testovanie v sandboxe](./sandbox-testing.md)
- [Konvencie API](./conventions.md)
- [Chyby a idempotencia](./errors-and-idempotency.md)
- [Webhooky](./webhooks.md)
- [Číselníky](./reference-tables.md)
- [API referencia](./api-reference/index.mdx)
