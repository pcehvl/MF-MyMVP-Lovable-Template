# Studentguide: spec-drevet utvikling med OpenCode og Lovable

Du jobber i to verktøy med hver sin oppgave:

| Verktøy | Du gjør | Du lager |
|---|---|---|
| **OpenCode** (produktløpet) | finner ut *hva* som skal lages og *hvorfor* | evidenskort, produktbeskrivelse, Stories med akseptansekriterier |
| **Lovable** (utførelsesløpet) | får appen bygget og sjekker at den gjør det spec-en sier | plan, app, akseptanse |

**Spec-en er kontrakten.** Lovable bygger bare det som står i en spec du har
overlevert. Vil du ha noe annet, endrer du spec-en i OpenCode og overleverer
den på nytt.

## 1. Oppstart (én gang)

1. Åpne malrepoet på GitHub og trykk **Use this template**. Gi repoet et navn
   for produktet ditt.
2. Klon repoet til maskinen din (GitHub Desktop går fint) og åpne mappen i
   OpenCode. Agenten `product-assistant` starter av seg selv.
3. Skriv `/mcps` i OpenCode, velg `lovable` og logg inn i nettleseren.
4. Lag et nytt prosjekt i Lovable. Velg Build mode og skriv `/harness-init`.

## 2. Produktløpet i OpenCode

1. **Evidens.** Gi agenten intervjuer, observasjoner og notater. Den lager ett
   kort per kilde i `docs/design-thinking/` (`E-001-…`). Store råfiler
   (opptak, PDF-er) legger du i `docs/design-thinking/_raw/` – de lastes
   aldri opp.
2. **Framing.** Fyll ut `product/product.md` sammen med agenten. Den spør om
   én ting om gangen og finner ikke på noe selv.
3. **Stories.** Lag Stories med akseptansekriterier:
   `- **AC1 — Kort tittel.** Det som skal kunne observeres.`
   Se eksempelet i [`examples/`](examples/).
4. **Ready.** Når du bekrefter at en Story er klar, får den `status: ready`
   og `revision: 1`.
5. **Overlever.** Skriv `/handover US-001`. Agenten sender spec-en til
   Lovable og sjekker at den kom fram uendret.

## 3. Utførelsesløpet i Lovable

1. **Planlegg:** i Plan mode, skriv `plan US-001`. Les planen, juster eller
   godkjenn.
2. **Lagre planen:** i Build mode, si «save the plan».
3. **Bygg:** `/build-story US-001`. Lovable bygger og viser hvordan hver ACn
   er oppfylt.
4. **Sjekk** preview selv.
5. **Godkjenn:** `/accept-story US-001`. Du svarer PASS eller FAIL for én ACn
   om gangen, og til slutt om du har nye ønsker. Ved FAIL får du en
   retteprompt – kjør den, og godkjenn på nytt.
6. **Publiser** når du vil: `/publish`, og trykk så Publish.

Ber du Lovable om noe som ikke står i spec-en, sier den nei. Det er med vilje:
ta ønsket med tilbake til OpenCode.

## 4. Tilbake til OpenCode

Skriv `/return US-001`. Agenten henter resultatet fra Lovable.

- Alt godkjent → du bekrefter, og Storyen blir `done`.
- Nye ønsker eller observasjoner → dere vurderer dem sammen, og du bestemmer:
  endre en Story (ny revisjon → `/handover` igjen), lage en ny Story, endre
  produktbeskrivelsen, eller ikke gjøre noe.

## 5. Gode vaner

- **Skriv ACn som noe man kan se.** «Listen viser…», «Når brukeren trykker…,
  vises…». Ikke «systemet er raskt».
- **Én ting per ACn.** Er det «og» i kriteriet, er det ofte to.
- **Smale Stories.** Én liten, hel funksjon er bedre enn mange halve.
- **Ikke rett i Lovable det som er feil i spec-en.** Rett spec-en.
- **Kreditter:** hver melding i Lovable koster. Overleveringer også.

## 6. Når noe går galt

| Problem | Gjør dette |
|---|---|
| `/handover` sier at Lovable-verktøyene mangler | Skriv `/mcps` og logg inn på `lovable` på nytt. |
| Innloggingen virker ikke | `/handover` skriver ut en blokk: lim den inn i Lovable-chatten i Build mode. |
| Lovable nekter å bygge | Planen er ikke godkjent, eller den er for en eldre revisjon: planlegg på nytt. |
| Noe annet | Spør veileder, og ta med hva du skrev og hva du fikk som svar. |
