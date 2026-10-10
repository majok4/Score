# Score – design

Skärmskisserna finns på designytan: https://claude.ai/artifact/7i5U86DmaiRudP59KHbUN1
Den har åtta klickbara mobilskärmar: matcher, matchsidans översikt, laguppställning och statistik, ligatabell, spelarprofil, favoriter med notiser och startsidan i ljust tema.
Här är reglerna bakom skisserna, så att appen byggs likadant.

Alla färgpar har kontrollerats mot minst 4,5:1 i kontrast för text. Ljust tema har därför mörkare metadata-grå och live-röd än mörkt tema.

## Känsla

Mörk som standard, som en arena i kvällsljus. Ljust tema finns för dagsljus.
Det enda som lyser är det som pågår: limegrönt betyder *ditt val* och korallrött betyder *live*.
Allt annat är lugnt och grått, så att resultaten syns först.

## Färger

| Token | Mörkt | Ljust | Används till |
|---|---|---|---|
| `--bg` | `#0B0E13` | `#F3F5F8` | Bakgrund |
| `--surface` | `#141922` | `#FFFFFF` | Kort, listor |
| `--surface-2` | `#1C2230` | `#E9EDF2` | Chips, inaktiva knappar |
| `--line` | `#262E3D` | `#DDE2EA` | Avdelare |
| `--text` | `#F2F4F8` | `#0D1117` | Rubriker, lag, resultat |
| `--text-2` | `#A3ACBC` | `#4B5566` | Brödtext |
| `--text-3` | `#7C8699` | `#5F6A7D` | Metadata (tid, land) |
| `--accent` | `#C8F031` | `#C8F031` med mörk text; `#4A7400` som text | Valt datum/flik, aktiv meny, följ |
| `--live` | `#FF5D3B` | `#B92B15` | Live-minut, live-filter |
| `--home` | `#5B8CFF` | `#2F63E0` | Hemmalag i statistik och diagram |
| `--away` | `#FF9F43` | `#D9730D` | Bortalag i statistik och diagram |

Hemma och borta är blått mot orange (inte rött mot grönt), och färgerna skiljer sig även i ljushet. Då går de att skilja åt även med färgblindhet.

### Betygsfärger (0–10)

Betyget visas alltid som siffra i en bricka med mörk text. Färgen är en extra signal, inte den enda.

| Betyg | Färg | Betydelse |
|---|---|---|
| 9,0–10 | `#4F8BFF` | Världsklass |
| 8,0–8,9 | `#19B5A5` | Utmärkt |
| 7,0–7,9 | `#3FBF5F` | Bra |
| 6,5–6,9 | `#D6B530` | Godkänt |
| 6,0–6,4 | `#EE8A2F` | Svagt |
| < 6,0 | `#E5484D` | Dåligt |

### Tabellplaceringar
En färgad markering vid placeringen: Europa-kval `--home`, Conference-kval `#19B5A5`, kvalspel `--away`, nedflyttning `#E5484D`. En förklaring visas under tabellen.

## Typsnitt

- **Barlow** (400/500/600/700) för all text i gränssnittet.
- **Barlow Condensed** (600/700) för resultat, minuter, betyg och sidrubriker. Smala siffror gör att resultat och tabeller får plats på smala skärmar.
- Siffror använder alltid `font-variant-numeric: tabular-nums`, så att kolumner inte hoppar när ställningen ändras.

| Stil | Storlek / vikt |
|---|---|
| Resultat i matchhuvud | Condensed 48/700 |
| Sidrubrik | Condensed 24/700 |
| Lagnamn i lista | 15/500 (600 för vinnaren) |
| Brödtext | 14/400 |
| Metadata | 12/500 |
| Flikar och chips | 13/600 |

## Mått och form

- Rutnät på 4 px. Sidomarginal 16 px.
- Matchrad 60 px. Rubrik för en liga 40 px. Bottenmeny 64 px plus säkerhetsmarginal.
- Hörnradie: kort 14 px, chips 999 px (helt runda), betygsbrickor 6 px.
- Tryckytor är minst 44×44 px, även stjärnan på en matchrad.
- Inga skuggor i mörkt läge; djup skapas med ytfärgerna `--surface` och `--surface-2`.

## Komponenter

- **Datumremsa**: veckodag och datum, idag markerat med limegrön bakgrund. Svep sidledes byter dag.
- **Sportchips**: rad som går att scrolla sidledes. Live-chipet ligger först och visar antal matcher.
- **Ligarubrik**: ligamärke, land och liganamn. Hela raden går till ligasidan.
- **Matchrad**: statuskolumn (48 px), två lagrader, resultat högerställt och följ-stjärna. Vinnaren visas i fetstil. Röda kort visas som en liten röd rektangel efter lagnamnet.
- **Matchhuvud**: ligger kvar högst upp och krymper till en 48 px resultatrad när man scrollar.
- **Flikar**: understruken aktiv flik i lime. Svep mellan flikarna.
- **Statistikrad**: etikett i mitten, värden ytterst, dubbel stapel under. Det bättre värdet får en färgad bakgrund.
- **Attack Momentum**: staplar per minut, hemma uppåt i blått och borta nedåt i orange, med mål markerade.
- **Plan för laguppställning**: båda lagen på samma plan. Spelaren visas som en cirkel med tröjnummer, betygsbricka och efternamn, plus ikoner för mål och kort.
- **Formbrickor**: V/O/F (vinst, oavgjort, förlust) som bokstav på färg.
- **Bottenmeny**: Matcher · Ligor · Favoriter · Sök · Mer. Aktiv flik får ikon och text i lime.
- **Lagmärken**: i designen används runda monogram i klubbfärg. I appen kommer märkena från dataleverantören.

## Rörelse

- Live-minuten pulserar lugnt (opacitet 1 → 0,4, 1,6 s).
- När ett mål görs blinkar resultatet i lime under 1,2 s, och telefonen vibrerar (om användaren slagit på det).
- Respektera `prefers-reduced-motion`: då stängs puls och blink av.
