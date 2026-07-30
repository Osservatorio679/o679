# Osservatorio679 — Icons & Transparency Widget

> **Sistema iconografico standardizzato per informative privacy GDPR**
> Licenza: **CC BY** — Osservatorio679
> GDPR Reference: Art. 12 §7, Art. 13, Art. 14

---

## 📌 Base URLs

| Tipo | URL |
|------|-----|
| **GitHub Pages (CDN)** | `https://osservatorio679.github.io/o679/` |
| **Raw GitHub (fallback)** | `https://raw.githubusercontent.com/Osservatorio679/o679/main/` |
| **Manifest JSON** | `/icons.json` |
| **Widget JS** | `/widget/o679-widget.js` |
| **Canale versione GIPD** | `/gipd-version.json` |
| **Demo live** | `/demo.html` |

---

## 🎨 Sistema a 3 stati (Trasparenza Dinamica)

| Stato | Colore | Significato GDPR |
|-------|--------|-----------------|
| `normal`    | 🟢 Teal `#00897B`  | Dati personali ordinari (Art. 4.1) |
| `sensitive` | 🟡 Giallo `#F9A825` | Dati particolari / sensibili (Art. 9) |
| `profiling` | 🔴 Rosso `#C62828`  | Dati profilizzanti (Art. 22) |
| `bn`        | ⬛ B/N             | Stampa monocromatica |

---

## 📂 Struttura repository

```
o679/
├── icons.json                    # Manifest completo icone + metadati GDPR
├── gipd-version.json             # Canale versione app GIPD
├── icons/
│   ├── normal/                   # Icone teal — dati normali
│   ├── sensitive/                # Icone giallo — dati sensibili
│   ├── profiling/                # Icone rosso — dati profilizzanti
│   └── bn/                       # Bianco/nero — stampa
├── widget/
│   └── o679-widget.js            # Widget trasparenza dinamica
├── demo.html                     # Demo live integrata
├── _headers                      # Cache + CORS (Cloudflare Pages)
└── .github/
    └── workflows/
        └── pages.yml             # Deploy automatico GitHub Pages
```

---

## ⚡ Quick Start — Widget Trasparenza

```html
<!-- 1. Includi il widget -->
<script src="https://osservatorio679.github.io/o679/widget/o679-widget.js"></script>

<!-- 2. Configura -->
<script>
const o679 = new Osservatorio679Widget({
  titolare:   "Azienda SRL",
  privacyUrl: "https://www.azienda.it/privacy",
  dpoEmail:   "dpo@azienda.it",
  position:   "bottom-right",
  lang:       "it"
});

// 3. Notifica a ogni trattamento
o679.notify({
  event:     'form-submit',
  dataLevel: 'normal',          // 'normal' | 'sensitive' | 'profiling'
  icon:      'dati-personali',
  message:   'Stiamo raccogliendo i tuoi dati per la registrazione.',
  canRevoke: true
});
</script>
```

---

## 🖼 Fetch icona singola

```javascript
// Utility statica
const url = Osservatorio679Widget.getIconUrl('trattamento-in-corso', 'sensitive');
// → https://osservatorio679.github.io/o679/icons/sensitive/trattamento-in-corso.png

// Oppure costruisci manualmente:
const BASE = 'https://osservatorio679.github.io/o679/';
const iconUrl = `${BASE}icons/${state}/${iconId}.png`;
```

---

## 📋 Icone disponibili

| ID | Label IT | Art. GDPR |
|----|----------|-----------|
| `titolare-trattamento` | Titolare del Trattamento | 13.1.a |
| `finalita` | Finalità del trattamento | 13.1.c |
| `tempi-conservazione` | Tempi di conservazione | 13.2.a |
| `trasferimento-dati` | Trasferimento dati | 13.1.f |
| `diritti` | Diritti dell'interessato | 13.2.b |
| `dpo` | DPO / RPD | 13.1.b |
| `categoria-dati` | Categoria dati | 13.1.c |
| `dati-personali` | Dati personali | 4.1 |
| `destinatari` | Destinatari | 13.1.e |
| `revoca-consenso` | Revoca consenso | 7.3 |
| `reclamo` | Reclamo al Garante | 13.2.d |
| `trattamento-in-corso` | Trattamento in corso | 12 |
| `fonte-del-dato` | Fonte del dato | 14.2.f |
| `bilanciamento-interessi` | Bilanciamento interessi | 6.1.f |
| `obbligo-conferimento` | Obbligo di conferimento | 13.2.e |
| `diritto-accesso` | Diritto di accesso | 15 |

---

## 🐍 Python / Backend

```python
import requests

BASE_URL = "https://osservatorio679.github.io/o679/"

# Carica manifest
manifest = requests.get(BASE_URL + "icons.json").json()

# Scarica icona
def get_icon(icon_id: str, state: str = "normal") -> bytes:
    url = f"{BASE_URL}icons/{state}/{icon_id}.png"
    return requests.get(url).content
```

---

## 🔄 Canale versione GIPD

L'app GIPD legge `gipd-version.json` per rilevare aggiornamenti disponibili.

URL (con fallback raw):
- Primario: `https://osservatorio679.github.io/o679/gipd-version.json`
- Raw:      `https://raw.githubusercontent.com/Osservatorio679/o679/main/gipd-version.json`

---

## 🔧 Script di manutenzione

Inclusi nel repo:

- `rename_icons.py` — copia/rinomina i PNG sorgente in slug canonici (`icons/normal/`, `icons/bn/`).
- `generate_colored_icons.py` — genera varianti `sensitive` (#F9A825) e `profiling` (#C62828) da `icons/normal/`.
- `verify_repo.py` — controlla la presenza dei 16 × 4 = 64 file e la coerenza col manifest.

```bash
pip install Pillow
python verify_repo.py
```

---

## 🚀 Rilasciare una nuova versione

1. Aggiorna `icons.json` (bump `version`) e/o `widget/o679-widget.js` (bump nell'header).
2. Se cambia GIPD, aggiorna `gipd-version.json` con la nuova release.
3. `git commit` + `git push origin main` — il workflow `pages.yml` rideploya in ~1 min.
4. Tag: `git tag v1.7.0 && git push origin v1.7.0`.
5. Opzionale: crea la Release GitHub allegando il pacchetto GIPD.

---

## 📜 Licenza

**Creative Commons Attribution (CC BY)** — Osservatorio679
Attribuzione richiesta: `Icone: Osservatorio679 — CC BY`

---

*Osservatorio679 — La comunicazione oltre al contenuto*
