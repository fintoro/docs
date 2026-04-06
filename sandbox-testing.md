---
title: "Testovanie v sandboxe"
---

Sandboxové firmy slúžia na izolované testovanie Fintoro API. Nepoužívajú samostatný host ani inú základnú cestu.
## Ako funguje sandbox model

- Pod jednou produkčnou firmou si môžete vytvoriť viacero sandboxových firiem.
- Produkčná aj sandboxová firma používajú rovnakú základnú URL `https://app.fintoro.sk/api/public/v1`.
- Pri requeste rozhoduje token firmy, nie host alebo iná cesta.
- Sandboxová firma má obmedzený prístup k niektorým sekciám Fintoro.
- Na testovanie Fintoro API však sandboxová firma funguje ako samostatné testovacie prostredie a má vlastné tokeny.

## Kde sandbox firmu vytvoriť

Vo Fintoro otvorte produkčnú firmu a prejdite do sekcie integrácií pre Fintoro API. Práve tu spravujete tokeny aj sandboxové firmy. Tam si vytvoríte sandboxové firmy pre konkrétne integračné scenáre, partnerov alebo testovacie prostredia.

Odporúčame vytvoriť samostatnú sandboxovú firmu pre každý väčší integračný scenár alebo partnera.

## Rovnaký host, iný token

```bash
# Produkčná firma
curl --request GET \
  --url https://app.fintoro.sk/api/public/v1/clients \
  --header 'Authorization: Bearer <production-company-token>'

# Sandboxová firma
curl --request GET \
  --url https://app.fintoro.sk/api/public/v1/clients \
  --header 'Authorization: Bearer <sandbox-company-token>'
```

URL je v oboch prípadoch rovnaké. Mení sa len token.

## Odporúčaný postup testovania

1. Vytvorte samostatnú sandboxovú firmu pre integračný scenár.
2. Vytvorte tokeny sandboxovej firmy podľa potreby read-only alebo write testov.
3. Otestujte lookupy, čítacie scenáre a následne zápisové scenáre s `Idempotency-Key`.
4. Pri porovnávaní s produkciou si držte oddelené tajné kľúče, logy a pomenovanie tokenov.
5. Po ukončení testovania sandboxovú firmu alebo jej tokeny upracte.
