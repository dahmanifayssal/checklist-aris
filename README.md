# Checklist ARIS — Nexum Advisory

A single-page web application for workplace risk analysis preparation (ARIS — *Analyse des Risques Intégrale et Systématique*), built for **Nexum Advisory** (M. Faïçal DAHMANI, Conseiller en Prévention).

## What it does

Guides a company contact through an 8-step checklist before a prevention advisor's site visit. At the end, the completed form is submitted automatically via Formspree and a PDF copy can be downloaded.

## Steps

| # | Section | Description |
|---|---------|-------------|
| 1 | Type d'analyse | Global company analysis or specific work-post analysis |
| 2 | Identification | Company name, contact, sector, workforce, visit date |
| 3 | Postes de travail | Work posts, tasks, ergonomic conditions |
| 4 | Analyses existantes | Existing risk analyses (ARIS, RPS, ergonomic, chemical…) |
| 5 | Dangers identifiés | Physical, chemical, biological, ergonomic and psychosocial hazards |
| 6 | Mesures en place | PPE, collective protections, procedures, safety training |
| 7 | Documents | Regulatory inspection reports and prevention documents checklist |
| 8 | Envoi | Summary review and submission |

## Tech stack

- Pure HTML/CSS/JavaScript — no build step, no dependencies to install
- [jsPDF](https://github.com/parallax/jsPDF) (CDN) for client-side PDF generation
- [Formspree](https://formspree.io) for form submission

## Usage

Open `index.html` directly in a browser — no server required.

```bash
open index.html
```

The form submits to a Formspree endpoint (`/f/mjgllodn`). To use this for a different account, replace that endpoint ID in the `sendEmail()` function.

## PDF export

After completing all 8 steps, clicking **"Terminer & Envoyer"** submits the form and triggers a PDF download named `Checklist_ARIS_<CompanyName>_<Date>.pdf`.

## Print support

The page includes a `@media print` stylesheet that hides the stepper and navigation and renders all sections for a clean paper printout.

## Contact

**Nexum Advisory** — M. Faïçal DAHMANI  
fayssal.dahmani@gmail.com
