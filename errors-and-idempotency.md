# Chyby, rate limiting a retry správanie

Táto kapitola vysvetľuje, aké chybové stavy môže Public API vracať, ako funguje rate limiting a kedy je retry bezpečný. Nájdete tu aj pravidlá pre `Idempotency-Key`, `X-Request-Id` a retry alebo deduplikáciu webhookov.

## HTTP chyby a error responses

- `401` alebo iná auth chyba znamená, že request nemá použiteľný bearer token. Typicky ide o chýbajúci, neplatný alebo revokovaný token.
- `403` znamená, že token alebo firma nemá oprávnenie na danú operáciu.
- `404` znamená, že resource v aktuálnom firemnom kontexte neexistuje.
- `429` znamená, že firma prekročila public API rate limit.
- `422` znamená validačnú chybu request payloadu alebo query parametrov.
- `500` znamená neočakávanú internú chybu.
- `503` znamená dočasne nedostupnú službu alebo závislosť.

Presný error shape a dostupné statusy si vždy overte na konkrétnom endpointu v API referencii.

Chybové odpovede na všetkých endpointoch pod `/api/public/*` vraciame ako JSON aj vtedy, keď request nepošle `Accept: application/json`. Toto pravidlo sa týka aj route-level `404` a neočakávaných interných chýb. Úspešné download endpointy, ktoré vracajú PDF alebo iný binárny obsah, tým nie sú dotknuté a naďalej vracajú svoj pôvodný `Content-Type`.

## Rate limiting

- Public API štandardne obmedzujeme na `120 requestov za minútu na firmu`.
- Limit sa počíta na úrovni firmy, nie na úrovni jednotlivých tokenov. Viac tokenov tej istej firmy teda zdieľa jeden spoločný budget.
- Všetky throttled endpointy vracajú hlavičky `X-RateLimit-Limit` a `X-RateLimit-Remaining` aj pri úspešných response-och.
- Pri prekročení limitu vraciame `429 Too Many Requests`; táto odpoveď nesie tie isté `X-RateLimit-Limit` a `X-RateLimit-Remaining` hlavičky a navyše pridáva `Retry-After` a `X-RateLimit-Reset`.
- Ak Vaša integrácia potrebuje vyšší limit, ponúkame aj individuálne enterprise nastavenia a obchodné podmienky. Kontaktujte nás na [info@fintoro.sk](mailto:info@fintoro.sk).

## Idempotency-Key a bezpečné retry

Pri create operáciách odporúčame posielať `Idempotency-Key`, ak ho daný endpoint podporuje. Ak zopakujete rovnaký create request s rovnakým key a rovnakým payloadom, backend vráti pôvodný výsledok namiesto druhého vytvorenia.

```bash
curl --request POST \
  --url https://app.fintoro.sk/api/public/v1/clients \
  --header 'Authorization: Bearer <company-token>' \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --header 'Idempotency-Key: client-import-2026-03-17-001' \
  --data '{
    "name": "Acme s.r.o.",
    "email": "api@example.com"
  }'
```

Ak rovnaký `Idempotency-Key` použijete s iným payloadom, endpoint request odmietne. Presný status a error payload si overte v dokumentácii konkrétneho endpointu.

Bez `Idempotency-Key` považujte automatický retry write requestov za rizikový, pretože pri sieťovej chybe alebo timeout-e nemusíte vedieť rozlíšiť, či backend zápis vykonal. Pri `500` a `503` považujte retry za bezpečný len vtedy, keď používate `Idempotency-Key` a nechcete riskovať duplicitný zápis.

## X-Request-Id a diagnostika

Fintoro vracia `X-Request-Id`, ktorý odporúčame ukladať do logov Vašej integrácie. Keď riešite incident, podporu alebo audit, práve tento identifikátor výrazne skracuje dohľadanie requestu a párovanie s našimi internými logmi.

## Webhook retry a deduplikácia

Ak používate webhooky, rátajte s modelom doručenia at-least-once:

- non-`2xx` odpoveď, timeout alebo sieťové zlyhanie spôsobia opakovaný pokus o doručenie,
- ten istý `webhook-id` môže prísť opakovane,
- prijímač webhookov musí deduplikovať a bezpečne spracovať znovu doručený event.

Pri prijímači webhookov odporúčame:

- ukladať `webhook-id` ešte pred následným spracovaním,
- vracať `2xx` až po úspešnom prijatí a zaradení vlastnej úlohy do fronty,
- vracať non-`2xx` len vtedy, keď chcete, aby Fintoro request zopakovalo.

## Implementačný checklist

- generujte `Idempotency-Key` tak, aby bol stabilný pre jeden business pokus o create,
- nemiešajte produkčné a sandbox tokeny ani ich request logy,
- pri validačných chybách pracujte s detailom response payloadu a opravte request podľa vrátených chýb,
- pri `429` rešpektujte `Retry-After` a rate-limit hlavičky namiesto agresívnych retry slučiek,
- ukladajte si `X-Request-Id` pre support, audit a incidenty,
- prijímač webhookov navrhnite idempotentne a s deduplikáciou podľa `webhook-id`.
