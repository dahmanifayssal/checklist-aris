# CLAUDE.md — Checklist ARIS

## Project overview

Single-file web app (`index.html`) for Nexum Advisory. It is a pre-visit risk analysis checklist (ARIS — Analyse des Risques Intégrale et Systématique) that a company fills out before a prevention advisor's site visit. The UI is in French targeting a Belgian audience.

## Architecture

Everything lives in one file: `index.html`. There is no build system, no package manager, no framework.

- **CSS** — inline in `<style>`, minified class names (`.cb`, `.tog`, `.item`, etc.)
- **JavaScript** — inline in `<script>` at the bottom, vanilla JS only
- **External dependency** — jsPDF loaded from cdnjs CDN (PDF generation)
- **Form backend** — Formspree endpoint `https://formspree.io/f/mjgllodn`

## Key JS globals

| Symbol | Purpose |
|--------|---------|
| `currentStep` | 0-indexed active step (0–7) |
| `totalSteps` | Always 8 |
| `typeAnalyse` | `'globale'` or `'poste'` — set in step 0 |
| `setType(type)` | Selects analysis type and shows/hides post detail fields |
| `goTo(step)` | Navigates backward only (stepper clicks) |
| `nextStep()` | Advances forward with validation |
| `prevStep()` | Goes back one step |
| `validate(step)` | Returns `false` and shows inline errors if required fields missing |
| `buildRecap()` | Populates the step-8 summary box |
| `sendAndConfirm()` | Shows confirmation screen, then calls `sendEmail()` |
| `sendEmail()` | POSTs form data as JSON to Formspree |
| `generatePDF()` | Builds and downloads a jsPDF document (called separately from send) |
| `val(id)` | Helper — returns trimmed value from any input/textarea/element |
| `tog(el)` | Toggles `.checked` class on custom checkbox divs |

## Section map

| Section ID | Step | Content |
|------------|------|---------|
| `#sec0` | 1 | Type d'analyse (globale / par poste) |
| `#sec1` | 2 | Identification entreprise |
| `#sec2` | 3 | Postes de travail et activités |
| `#sec3` | 4 | Analyses existantes |
| `#sec4` | 5 | Dangers identifiés |
| `#sec5` | 6 | Mesures de prévention |
| `#sec6` | 7 | Documents disponibles |
| `#sec7` | 8 | Récapitulatif & envoi |
| `#page-confirm` | — | Confirmation screen shown after send |

## Conventions

- Field IDs follow the pattern `f_<field>` (identification fields) and `q<section>_<name>` (checklist fields).
- Custom checkboxes are `<div class="cb">` elements toggled with `tog(this)` — not native `<input type="checkbox">`. Native checkboxes are used only for the multi-select "analyses spécifiques" list (`input[name="spec"]`).
- Colors: dark brown `#4A1B0C`, orange `#D85A30`, light peach `#FAECE7` — keep all UI changes within this palette.
- The app is print-friendly: `@media print` hides stepper, nav, footer, and progress bar and forces all sections visible.
- Language: all user-facing strings are in French. Keep it that way.

## Things to be careful about

- `validate()` only enforces required fields on steps 0 and 1. Steps 2–7 have no mandatory fields.
- `goTo(step)` blocks forward navigation — users can only go back via the stepper. Forward movement is exclusively through the Next button.
- `generatePDF()` is defined but not wired to a button in the current UI — it exists for potential future use or manual invocation.
- The Formspree endpoint ID (`mjgllodn`) is hardcoded in `sendEmail()`. Do not change it without confirming with the project owner.
- jsPDF is loaded from a CDN; the app has no offline fallback for PDF generation.
