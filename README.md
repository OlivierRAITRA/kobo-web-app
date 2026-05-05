# XLSForm parser (backend)

Ce module lit un **XLSForm** (`.xlsx`) et transforme les feuilles **`survey`** et **`choices`** en JSON exploitable pour générer un formulaire dynamique.

## Installation

Dans `kobo-web-app/backend` :

```bash
npm install
```

## Utilisation en code

```js
import { parseXLSForm } from "./src/index.js";

const form = parseXLSForm("../xlsform/BILAN ET ACCOMPAGNEMENT V4.xlsx");
console.log(form.settings.title);
console.log(form.survey[0]);
```

### Structure du JSON

- `settings`: infos générales (`title`, `id`, `version`, `instance_name`)
- `choices`: index par `list_name` → tableau d’options `{ name, label, media }`
- `survey`: arbre de questions/groupes/répétitions
  - `type`: type XLSForm (ou `group` / `repeat`)
  - `name`
  - `label` / `hint`: objets i18n (`default`, `fr`, `en`, ...)
  - `required`, `readOnly`, `appearance`, `relevant`, `constraint`, `calculation`, `default`
  - `choice_list`: nom de liste si `select_one` / `select_multiple`
  - `choices`: options résolues (si trouvées)
- `warnings`: incohérences (ex: `end group` sans `begin group`)

## CLI

```bash
node bin/xlsform-to-json.js -i "../xlsform/BILAN ET ACCOMPAGNEMENT V4.xlsx" -o "../xlsform/form.json"
```

