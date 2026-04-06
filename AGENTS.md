# Docs repo rules

- Tento repozitár hostuje Mintlify dokumentáciu pre Fintoro Public API.
- Predvolené jazykové vetvy:
  - koreň repo = slovenský obsah,
  - `en/` = anglický obsah.
- Mintlify nepodporuje locale code `sk`, preto je slovenská vetva v natívnom language switcheri dočasne vedená ako `cs`.
- OpenAPI source of truth v tomto repo:
  - `openapi.yaml`
  - `en/openapi.yaml`
- Human guide pages udržiavaj v oboch jazykoch obsahovo zladené.
- Keď meníš navigáciu alebo branding, uprav `docs.json`.
- Keď meníš guide page linky, preferuj interné Mintlify routy pred odkazom na raw YAML súbor.
- Lokálne preview:
  - `mint dev`
  - `mint broken-links`
