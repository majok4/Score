# Score – plan

En egen livescore-app i stil med SofaScore, byggd för mobilen först.
Det här dokumentet beskriver **vad appen ska innehålla**, **hur den byggs** och **i vilken ordning**.
Hur den ska se ut beskrivs i [DESIGN.md](./DESIGN.md) och visas som klickbara skärmar på designytan.

---

## 1. Vad SofaScore gör

SofaScore är i grunden fyra saker:

1. **Livescore**: 25 sporter och över 5 000 ligor, med resultat som uppdateras direkt.
2. **Djupstatistik per match**: händelser, laguppställningar, över 300 statistikvärden, *Attack Momentum* (en graf över vilket lag som trycker på), skottkarta med xG, värmekartor och box score.
3. **Spelarbetyg 0–10**: varje spelare får ett betyg live under matchen, och betyget summeras över säsongen.
4. **Personligt flöde**: favoritlag, -spelare och -ligor, med noggranna push-notiser.

Utöver det finns tabeller, slutspelsträd, veckans lag, inbördes möten (H2H), odds, röstningen "vem vinner?", TV-tablåer, höjdpunkter, widgets och stöd för klockor.

**Var SofaScore är svagt på mobil**, och där vi kan bli bättre:

- Helskärmsannonser och omdirigeringar är det vanligaste klagomålet i recensionerna.
- Mycket information trängs ihop och det är svårt att hitta rätt.
- Laguppställningar och skadeinformation stämmer inte alltid.
- Odds och spel syns överallt och går inte att stänga av helt.
- Appen är tung: den laddar mycket och drar mycket data.

## 2. Mål: minst lika bra på mobil

| Princip | Vad det betyder konkret |
|---|---|
| **Snabb** | Första skärmen visas på under 1,5 s på 4G. En live-ändring syns i appen inom 5 s. |
| **En hand** | All navigering ligger i tumzonen längst ner. Inga viktiga knappar högst upp. |
| **Lugn** | Inga helskärmsannonser. Odds är avstängda som standard och kräver att man själv slår på dem (18+). |
| **Svep** | Svep sidledes byter dag i matchlistan och flik på matchsidan. |
| **Offline** | Appen kan installeras (PWA). Den senast hämtade datan visas utan nät, märkt med "uppdaterad 14:32". |
| **Svensk först** | Appen är på svenska med engelska som alternativ. Allsvenskan, Damallsvenskan, Superettan och SHL finns från start. |
| **Tillgänglig** | Text har minst 4,5:1 i kontrast och tryckytor är minst 44 px. Betyg visas alltid med siffra, inte bara med färg. |

---

## 3. Innehåll: alla funktioner

Prioritet: **MVP** = första versionen · **v1** = strax efter · **v2** = senare.

### 3.1 Matcher (startsidan)
- **MVP** Datumremsa med idag i mitten; svep mellan dagar; tryck för kalender
- **MVP** Sportväljare som chips (fotboll först, fler sporter i v2)
- **MVP** Live-filter som visar antal pågående matcher ("Live 12")
- **MVP** Matcher grupperade per turnering med land, flagga och ligamärke; fästa ligor visas överst
- **MVP** Matchrad: tid eller minut (pulserande vid live), lag och märken, ställning, röda kort, sammanlagt resultat i cuper, stjärna för att följa
- **MVP** Sektionen "Mina matcher" överst (favoritlag och följda matcher)
- **MVP** Live-uppdatering utan omladdning, plus dra-för-att-uppdatera
- **v1** Sortering efter avsparkstid eller efter liga
- **v1** Kompakt eller luftig radhöjd
- **v2** Odds på raden (valbart)

### 3.2 Matchsida
Huvudet ligger kvar högst upp: lag, ställning, minut, målskyttar och halvtidsresultat. När man scrollar krymper det till en tunn resultatrad.

| Flik | Innehåll | Prio |
|---|---|---|
| **Översikt** | Händelser (mål, kort, byten, VAR, missad straff), Attack Momentum, matchens bästa spelare, "vem vinner?"-röstning, arena, domare, publik, TV-kanal | MVP (momentum v1) |
| **Laguppställning** | Formation på en plan med betyg på varje spelare, avbytare, skadade/avstängda, tränare, lagets snittbetyg | MVP |
| **Statistik** | Bollinnehav, xG, skott (på mål / utanför / blockade), stora chanser, hörnor, passningar, dueller, frisparkar, kort; uppdelat per halvlek | MVP |
| **Tabell** | Ligatabell med båda lagen markerade och live-tabell under pågående omgång | MVP |
| **H2H** | Inbördes möten, form (senaste 5), hemma/borta-form, målsnitt | MVP |
| **Spelare** | Box score-tabell som går att sortera på betyg, mål, skott, passningar osv. | v1 |
| **Skottkarta** | Varje skott på planen med xG-storlek och utfall | v1* |
| **Kommentarer** | Minut-för-minut-text | v1 |
| **Odds** | Bara om användaren slagit på det (18+) | v2 |
| **Media** | Länkar till höjdpunkter | v2 |

- **MVP** Följ match: notis-klocka i huvudet
- **v1** Dela ett snyggt matchkort som bild till Instagram, sms eller WhatsApp
- **v1** Haptisk puls vid mål i följd match (vibration på Android)

\* Skottkarta och värmekarta kräver koordinatdata (x/y per händelse). Det finns bara hos dyrare dataleverantörer, se avsnitt 4.

### 3.3 Spelarbetyg
- Skala 0–10 med en decimal, live under matchen.
- Färgskala med sex steg (se DESIGN.md). Siffran visas alltid.
- **MVP:** vi använder leverantörens betyg.
- **v2:** vår egen betygsmodell, som viktar händelser (mål, nyckelpassningar, vunna dueller, misstag) efter position.

### 3.4 Lagsida
- **MVP** Översikt: nästa match, form, tabellplacering, senaste resultat
- **MVP** Matcher (kommande och spelade), tabell, trupp med position och nummer
- **v1** Säsongsstatistik, bästa spelare efter betyg, skador
- **v2** Transfers, tränare, arena

### 3.5 Spelarsida
- **v1** Profil: ålder, längd, fot, position, nummer, land, klubb, kontrakt
- **v1** Säsong: matcher, mål, assist, snittbetyg och betyg per match som stapeldiagram
- **v1** Attributöversikt: radar med anfall, teknik, taktik, försvar och kreativitet
- **v2** Säsongsvärmekarta\*, karriär/transfers, landslag, jämför två spelare

### 3.6 Turnering / liga
- **MVP** Tabell: total/hemma/borta, form, färgade kvalplatser (Europa, kval, nedflyttning)
- **MVP** Omgångar: välj omgång, se matcher
- **v1** Toppspelare: betyg, mål, assist, hållna nollor
- **v1** Veckans lag (formation med bästa betyg)
- **v1** Slutspelsträd för cuper och slutspel
- **v2** Säsongsstatistik per lag, xG-tabell

### 3.7 Favoriter och notiser
- **MVP** Följ lag, ligor och matcher. Sparas lokalt, ingen inloggning krävs.
- **v1** Följ spelare
- **v1** Webb-push per objekt med val per typ: matchstart, mål, röda kort, halvtid/slut, laguppställning släppt
- **v1** Tysta timmar (till exempel 23–07)
- **v2** Konto för synk mellan enheter (valfritt)

### 3.8 Sök
- **MVP** Lag och ligor
- **v1** Spelare och tränare, senaste sökningar, populärt just nu

### 3.9 Inställningar ("Mer")
- **MVP** Tema (system/ljust/mörkt), språk (sv/en), tidszon
- **v1** Notiser, startsport, radhöjd, datasparläge
- **v2** Odds på/av och oddsformat

### 3.10 Mobilfunktioner där vi ska vara bättre än SofaScore
- **MVP** Installerbar PWA på hemskärmen, i helskärm och med egen ikon
- **MVP** Offline-cache med tydlig "senast uppdaterad"-tid
- **MVP** Svepgester för datum och flikar
- **v1** Mini-resultatrad för en följd match som följer med när man bläddrar i appen
- **v1** Datasparläge: inga lagmärken, glesare uppdatering
- **v2** Hemskärmswidget, Live Activities på iPhone och klocka. Kräver en native app (Capacitor), fas 3.

### 3.11 Senare (v2+)
- Fler sporter: ishockey (SHL), tennis, basket, handboll, innebandy, bandy
- Community: röstning, rapportera fel i laguppställningar
- Tippning eller fantasy mellan vänner
- Nyheter och höjdpunkter

---

## 4. Teknik

### Upplägg

```
┌──────────────┐  HTTPS/JSON   ┌───────────────────┐   API-nyckel   ┌─────────────────┐
│  PWA (React) │ ────────────▶ │  Proxy + cache     │ ─────────────▶ │  Dataleverantör │
│  i mobilen   │ ◀──────────── │  (serverless)      │ ◀───────────── │  (t.ex. API-    │
└──────┬───────┘   Web Push    │  · cachar svar     │                │   Football)     │
       │       ◀────────────── │  · pollar live     │                └─────────────────┘
       │                       │  · skickar notiser │
       ▼                       └───────────────────┘
  localStorage / IndexedDB
  (favoriter, offline-data)
```

- **Frontend:** React, TypeScript och Vite som PWA (`vite-plugin-pwa`), React Router, TanStack Query (cache och polling), Zustand (favoriter och inställningar). Eget litet designsystem i CSS-variabler. Diagram (momentum, radar, staplar) ritas som egen SVG, utan tunga bibliotek.
- **Proxy:** en liten serverless-tjänst (Hono på Cloudflare Workers eller Vercel). Den gömmer API-nyckeln och cachar svar så att tusen användare kostar lika många anrop som en: live-matcher 10–15 s, dagens matcher 1 min, tabeller 10 min, profiler 24 h. Den kör också ett schemalagt jobb som pollar live-matcher och skickar webb-push.
- **Datalager i appen:** ett gränssnitt, `DataProvider`, med två implementationer:
  - `MockProvider` simulerar live-matcher med mål, kort och byten. Den används för utveckling, demo och tester, så att hela appen kan byggas utan API-nyckel.
  - `ApiFootballProvider` (eller annan leverantör) är den riktiga datan, via proxyn.
- **Native senare:** samma kod paketeras med Capacitor för App Store och Google Play när widgets och Live Activities behövs.

### Datakälla – det största beslutet

SofaScore samlar in mycket av sin data själva. Vi måste köpa den.

| Leverantör | Kostnad (ungefär, verifieras) | Täcker | Saknar |
|---|---|---|---|
| **API-Football** (api-sports.io) | Gratis 100 anrop/dag · Pro ca 19 USD/mån för 7 500/dag · Ultra ca 29 USD för 75 000/dag | Live, händelser, laguppställningar, statistik, **spelarbetyg**, tabeller, H2H, odds, Allsvenskan m.fl. | Skott- och värmekartor (inga x/y-koordinater), Attack Momentum. Varje sport kräver en egen prenumeration. |
| **Sportmonks** | Dyrare, betalt per ligapaket | Mycket fotbollsdata, xG och pressindex (premium) | Dyrt med många ligor |
| **football-data.org** | Gratis för cirka 12 toppligor | Resultat och tabeller | Laguppställningar och djupstatistik i gratisnivån, Allsvenskan |
| **SofaScores eget API** | – | – | **Används inte.** Det är inofficiellt och bryter mot deras villkor. |

**Rekommendation:**
- Börja med **mock-data** och bygg hela appen på den.
- Koppla sedan på **API-Football Pro** (ca 19 USD/mån) med proxy-cache.
- Attack Momentum räknar vi fram själva ur händelser och statistik (skott, hörnor, farliga anfall). Det blir en uppskattning.
- Skott- och värmekartor tas med först om vi uppgraderar till en leverantör med koordinatdata.

### Kvalitet
- Enhetstester (Vitest) för datalagret och betygsfärger. Komponenttester för matchraden och tabellen.
- End-to-end-tester (Playwright) i mobilstorlek, 390×844, på huvudflödena.
- Prestandabudget: JS under 150 kB gzip i första laddningen, Lighthouse mobil över 90.

---

## 5. Navigation

Bottenmeny med fem flikar: **Matcher · Ligor · Favoriter · Sök · Mer**

```
/                         Matcher (idag)
/datum/2026-10-10         Matcher för ett datum
/match/:id                Matchsida  (?flik=oversikt|lag|statistik|tabell|h2h)
/lag/:id                  Lagsida
/spelare/:id              Spelarsida
/liga/:id                 Turnering  (?flik=tabell|matcher|spelare|slutspel)
/ligor                    Bläddra ligor per land
/favoriter                Mina lag, ligor, spelare och matcher
/sok                      Sök
/mer                      Inställningar
```

Alla sidor har egna länkar som går att dela, och bakåt-svep fungerar som förväntat.

---

## 6. Faser

| Fas | Innehåll | Klart när |
|---|---|---|
| **0 · Plan och design** (nu) | Den här planen, designsystem och skärmskisser | Du har godkänt riktningen |
| **1 · MVP** | Matchlista, matchsida (översikt, laguppställning, statistik, tabell, H2H), ligatabell, favoriter lokalt, mörkt/ljust tema, PWA och offline, mock-data och första riktiga datakällan | Allsvenskan går att följa live i mobilen |
| **2 · Djup** | Lag- och spelarsidor, sök, Attack Momentum, webb-push, delbara matchkort, box score, kommentarer | Paritet med SofaScore för fotboll (utom koordinatdata) |
| **3 · Bredd** | Ishockey (SHL), tennis, basket, handboll, innebandy; native-app via Capacitor med widgets | Fler sporter och appbutiker |
| **4 · Eget** | Egen betygsmodell, community-röstning, tippning bland vänner | Det som SofaScore inte har |

---

## 7. Beslut som behövs

1. **Datakälla och budget:** OK att börja med mock-data och sedan API-Football Pro (ca 19 USD/mån)? Eller ska vi bara använda gratis källor?
2. **Plattform:** PWA först, som fungerar på iPhone och Android direkt via en länk (rekommenderas)? Eller native-app direkt?
3. **Ligor först:** Allsvenskan, Damallsvenskan, Superettan, Premier League, La Liga, Serie A, Bundesliga, Champions League. Något att lägga till eller ta bort?
4. **Namn:** "Score" som arbetsnamn?
