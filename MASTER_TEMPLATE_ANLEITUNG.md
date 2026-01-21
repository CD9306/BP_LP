# Master-Template Anleitung

## Übersicht

Diese Landing Page ist nun als **Master-Template** konfiguriert. BestPrime-Daten bleiben fix, während Vertriebspartner-Daten zentral verwaltet und dynamisch eingebunden werden.

## Struktur

```
src/
├── data/
│   └── partnerData.ts          # Zentrale VP-Daten (HIER ANPASSEN!)
├── components/
│   ├── FooterLegal.tsx         # Footer mit Impressum/Datenschutz-Links
│   └── LegalModal.tsx          # Wiederverwendbares Modal
├── legal/
│   ├── Impressum.tsx           # Impressum (BestPrime fix + VP variabel)
│   └── Datenschutz.tsx         # Datenschutz (BestPrime fix + VP variabel)
└── App.tsx                      # Hauptseite mit Footer-Einbindung
```

## Vertriebspartner-Daten anpassen

### 1. Datei öffnen: `src/data/partnerData.ts`

### 2. Pflichtfelder ausfüllen:
```typescript
{
  customerNumber: "VP-12345",           // Kundennummer
  companyOrName: "Firmenname",          // Firma oder Name
  street: "Straße 123",                 // Straße + Hausnummer
  zip: "12345",                         // PLZ
  city: "Stadt",                        // Stadt
  email: "kontakt@firma.de"             // E-Mail
}
```

### 3. Optionale Felder:
Alle optionalen Felder werden automatisch ausgeblendet, wenn sie leer sind:

```typescript
{
  legalRepresentative: "Max Mustermann",  // Geschäftsführer
  country: "Deutschland",                  // Land
  phone: "+49 123 456789",                // Telefon
  mobile: "+49 160 123456789",            // Mobil
  website: "www.firma.de",                // Website
  vatId: "DE123456789",                   // USt-ID
  taxId: "12/345/67890",                  // Steuernummer
  registerCourt: "Amtsgericht Stadt",     // Registergericht
  registerNumber: "HRB 12345",            // Handelsregisternummer
  calendlyLink: "https://calendly.com/...", // Calendly-Link
  privacyContactName: "Max Mustermann",   // Datenschutzkontakt Name
  privacyContactEmail: "datenschutz@firma.de" // Datenschutzkontakt E-Mail
}
```

## Funktionsweise

### Impressum & Datenschutz

- **Footer-Links**: Klick auf "Impressum" oder "Datenschutz" öffnet Modal
- **Modal-Steuerung**:
  - Schließen via X-Button
  - Schließen via ESC-Taste
  - Schließen via Klick außerhalb
  - Body Scroll Lock während Modal geöffnet
- **Responsive**: Mobile-optimiert, max-width 900px, max-height 85vh

### Datenfluss

1. **Zentrale Daten**: `partnerData` in `src/data/partnerData.ts`
2. **Impressum**: Nutzt `partnerData` für VP-Bereich
3. **Datenschutz**: Nutzt `partnerData` für Verantwortlicher & Kontakt
4. **Automatisch**: Leere optionale Felder werden nicht angezeigt

### BestPrime-Daten (fix)

Die folgenden Daten bleiben in allen Impressum/Datenschutz-Texten fest:

```
BestPrime GmbH
Geschäftsführer: Thomas Bayer
Werner-von-Siemens-Str. 2-6
86159 Augsburg
Deutschland

Telefon: +49 821 780 53 69 - 0
E-Mail: info@bestprime.de
Web: www.bestprime.de

Amtsgericht Augsburg, HRB 35086
USt-ID: DE323751029
```

## Vorbereitung für API-Integration

Die aktuelle Struktur ist bereits API-ready:

### Aktuell (statisch):
```typescript
import { partnerData } from '../data/partnerData';
```

### Zukünftig (dynamisch):
```typescript
// In App.tsx oder Context
const [partnerData, setPartnerData] = useState<PartnerData | null>(null);

useEffect(() => {
  fetch('/api/partner-data')
    .then(res => res.json())
    .then(data => setPartnerData(data));
}, []);
```

**Wichtig**: Impressum und Datenschutz-Komponenten müssen NICHT geändert werden!

## Typensicherheit

TypeScript-Typ `PartnerData` stellt sicher, dass alle Pflichtfelder vorhanden sind:

```typescript
export interface PartnerData {
  // Required
  customerNumber: string;
  companyOrName: string;
  street: string;
  zip: string;
  city: string;
  email: string;

  // Optional
  legalRepresentative?: string;
  country?: string;
  phone?: string;
  mobile?: string;
  website?: string;
  vatId?: string;
  taxId?: string;
  registerCourt?: string;
  registerNumber?: string;
  whatsappLink?: string;
  linkedinLink?: string;
  calendlyLink?: string;
  privacyContactName?: string;
  privacyContactEmail?: string;
}
```

## Fallback-Logik

Datenschutz-Komponente hat intelligente Fallbacks:

```typescript
const privacyContactName = partnerData.privacyContactName || partnerData.companyOrName;
const privacyContactEmail = partnerData.privacyContactEmail || partnerData.email;
const contactPhone = partnerData.phone || partnerData.mobile || '';
```

## Testen

1. **Entwicklungsserver starten**: `npm run dev`
2. **Impressum-Link**: Im Footer auf "Impressum" klicken
3. **Datenschutz-Link**: Im Footer auf "Datenschutz" klicken
4. **Modal-Funktionen testen**:
   - Schließen mit X
   - Schließen mit ESC
   - Schließen mit Klick außerhalb
   - Scrollen im Modal
5. **Responsive testen**: Mobile, Tablet, Desktop

## Deployment

Nach Anpassung der `partnerData.ts`:

```bash
npm run build
```

Build-Artefakte befinden sich in `dist/` und können deployed werden.

## Wartung

### Neue VP-Landingpage erstellen:
1. `src/data/partnerData.ts` anpassen
2. `npm run build`
3. Deployen

### BestPrime-Daten ändern:
1. `src/legal/Impressum.tsx` - BestPrime-Bereich anpassen
2. `src/legal/Datenschutz.tsx` - BestPrime-Bereich anpassen

## Hinweise

- **Keine zusätzlichen UI-Frameworks**: Alles mit React + Tailwind CSS
- **TypeScript**: Vollständige Typsicherheit
- **Keine toten Imports**: Alle Imports werden genutzt
- **Leere Felder**: Werden automatisch ausgeblendet
- **SEO-freundlich**: Semantic HTML
- **Accessibility**: ARIA-Labels, Keyboard-Navigation

## Support

Bei Fragen zur Anpassung oder technischen Problemen wenden Sie sich an:
- BestPrime Support: info@bestprime.de
- Telefon: +49 821 780 53 69 - 0
