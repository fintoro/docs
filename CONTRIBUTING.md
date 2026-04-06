# Contributing

## Scope

Tento repozitár obsahuje Mintlify dokumentáciu pre Fintoro API. Zmeny v guide pages, navigácii a OpenAPI specoch udržiavajte obsahovo zladené medzi slovenskou a anglickou vetvou.

## Local preview

```bash
mint dev
```

Preview je dostupné na `http://localhost:3000`.

## Required checks

Pred odovzdaním zmien spustite:

```bash
mint validate
mint broken-links
```

## Content rules

- Interné odkazy smerujte na Mintlify routy bez `.md` a `.mdx`.
- Link na raw OpenAPI používajte len tam, kde má ísť o download.
- Keď meníte API kontrakt, dorovnajte zmenu v `openapi.yaml` aj `en/openapi.yaml`.
