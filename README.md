# Fintoro Public API docs

Mintlify dokumentácia pre Fintoro Public API.

## Lokálny vývoj

Nainštalujte Mintlify CLI:

```bash
npm i -g mint
```

V koreňovom priečinku docs spustite preview:

```bash
mint dev
```

## Kontroly

Pred odovzdaním zmien spustite:

```bash
mint validate
mint broken-links
```

## Štruktúra

- koreň repo obsahuje slovenskú vetvu, ktorá je v Mintlify switcheri vedená pod locale kódom `cs`,
- `en/` obsahuje anglickú vetvu,
- `openapi.yaml` a `en/openapi.yaml` sú source of truth pre API reference.
