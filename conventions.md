# Konvencie API

Táto sekcia zhŕňa pravidlá, ktoré sa opakujú naprieč Public API v1.

## Requesty

- Public API používa JSON requesty a JSON response payloady.
- Business payloady v Public API v1 používajú názvy polí v camelCase.
- Všetky requesty sú cez bearer token viazané na konkrétnu firmu.
- Pri create operáciách odporúčame používať `Idempotency-Key`. Nie je povinný, ale výrazne zvyšuje bezpečnosť integrácie, pomáha predchádzať duplicitám pri opakovaných pokusoch a robí zápisové scenáre robustnejšími.
- Voliteľný header `Accept-Language` môžete poslať pri ktoromkoľvek Public API requeste.

## Lokalizácia odpovedí

- `Accept-Language` sa vyhodnocuje naprieč celým Public API.
- Hlavička lokalizuje fixné lookupy a rovnaké systémové objekty, keď sa vracajú vnorené v list, detail, create alebo update odpovediach.
- Typicky ide o krajiny, meny, spôsoby dodania a ďalšie podobné systémové lookupy.
- Nelokalizujú sa používateľské dáta, napríklad názvy klientov, dodávateľov, skladov alebo textové poznámky.
- Validačné chyby používajú rovnaký header. Ak pre zvolený jazyk nie je dostupná vlastná validačná sada, validačné chyby fallbackujú do angličtiny. Pri češtine fallbackujú do slovenčiny.
- Ak hlavičku nepošlete alebo pošlete nepodporovanú hodnotu, odpoveď sa vráti v predvolenom jazyku Fintoro Public API.
- Response vracia `Content-Language` s jazykom vybraným pre request.

Podporované request language tagy:

| Jazyk | Odporúčaný tag | Jazyk validačných chýb |
| --- | --- | --- |
| Slovenčina | `sk` | Slovenčina |
| Angličtina | `en` | Angličtina |
| Čeština | `cs` | Slovenčina |
| Nemčina | `de` | Angličtina |
| Estónčina | `et` | Angličtina |
| Španielčina | `es` | Angličtina |
| Francúzština | `fr` | Angličtina |
| Chorvátčina | `hr` | Angličtina |
| Maďarčina | `hu` | Angličtina |
| Taliančina | `it` | Angličtina |
| Litovčina | `lt` | Angličtina |
| Holandčina | `nl` | Angličtina |
| Poľština | `pl` | Angličtina |
| Portugalčina | `pt` | Angličtina |
| Rumunčina | `ro` | Angličtina |
| Srbčina | `sr` | Angličtina |
| Ruština | `ru` | Angličtina |
| Slovinčina | `sl` | Angličtina |
| Ukrajinčina | `uk` | Angličtina |

## Response model

- Zoznamové endpointy wrapujú výsledky do objektu s `data` a prípadnými ďalšími metadátami, napríklad `paginator`.
- Detailné a create/update endpointy vracajú business objekt priamo bez obalu.
- Pri debugovaní si ukladajte `X-Request-Id` z response headera.

## Reakcia na zmeny a synchronizácia

- Ak potrebujete reagovať na create, update alebo delete udalosti a držať externý systém priebežne zosynchronizovaný, implementujte webhooky.
- Periodické dotazovanie nie je odporúčaný mechanizmus na detekciu zmien. Používajte ho len na prvotný import, backfill po incidente alebo kontrolné dorovnanie.
- Odporúčaný model je: webhook použijete ako trigger a detail resource-u si následne načítate cez Public API.
- Webhook payloady zámerne nevracajú celý snapshot resource-u. Zdrojom pravdy pre detail zostáva API referencia konkrétneho endpointu.
- Pri delete eventoch rátajte s tým, že detail endpoint už nemusí resource vrátiť.

## Lookupy a číselníky

- Fixné lookupy vracajú celý dataset bez filtrovania a stránkovania.
- ID fixných lookupov sú stabilné v rámci tejto verzie API.
- Rýchly ľudský prehľad stabilných ID nájdete aj v [referenčných tabuľkách](./reference-tables.md).

## Poznámky ku konkrétnym resource-om

- Pri dokladových create flowoch sa časť hodnôt môže doplniť z klienta, firemných nastavení alebo systémových resolverov. Presné pravidlá vždy overte v schéme konkrétneho endpointu.
- `contact-activity-logs` prílohy používajú dvojkrokový upload flow cez samostatný upload endpoint a následné použitie upload tokenov.
- Dokladové resource-y ponúkajú samostatné endpointy na stiahnutie PDF alebo `pdfDownloadUrl` v odpovedi.
- Väčšina upraviteľných business resource-ov môže zároveň emitovať webhook eventy. Presný katalóg eventov nájdete v [Webhookoch](./webhooks.md).

## Verzovanie a kontrakt

- Táto dokumentácia popisuje Public API v1 pod `/api/public/v1`.
- Ak potrebujete presnú podobu payloadu, enumov alebo validačných pravidiel, zdrojom pravdy je [API referencia](./api-reference/index.mdx).
