# FE-Buddy Alias Command Guide

These alias commands are made by [FE-Buddy](https://github.com/Nikolai558/FE-BUDDY) every AIRAC cycle.

**Contents:** [How to read this guide](#how-to-read-this-guide) · [In-Scope Reference (ISR)](#in-scope-reference-isr) · [Data Display](#data-display) · [Chart Recall](#chart-recall)

## How to read this guide

- Plain text, typed exactly as shown. For example: "`.apt`"
- `<airport ID>` Angle brackets: replace them, and what is inside them, with the real value.
- `[page]` Square brackets: optional. Replace them in the same way, or leave them out.
- **Commands are not case-sensitive**
  - These two work the same:
    - `.aptdtw`
    - `.aptDTW`
- **Airport IDs**
  - Use the FAA ID, not the ICAO ID, unless the command says otherwise. For example, `DTW`, not `KDTW`.

## In-Scope Reference (ISR)

Information cards for airports, NAVAIDs and airlines (may include Virtual Airlines, if your FE has set it up).

| Syntax | Description | Example |
| --- | --- | --- |
| `.apt`<br>`<FAA or ICAO airport ID>` | Shows the airport's card:<br>• FAA and ICAO IDs, name, tower type and ARTCC<br>• Longest runway, elevation and traffic pattern altitude<br>• FSS, CTAF and weather frequency<br>• Attended hours (for towered airspace only)<br>• Class of airspace, with the hours it is in effect | `.aptDTW`<br>`.aptKDTW` |
| `.nav`<br>`<NAVAID ID or name>` | Shows the NAVAID's card:<br>• ID, name, type and frequency<br>• The ARTCCs it is in, for high and low altitude airspace<br>*When entering the name, leave out spaces and special characters.*<br>*When several NAVAIDs share the ID or name, the card lists each of them.* | `.navCGT`<br>`.navCHICAGOHEIGHTS` |
| `.id`<br>`<operator 3LD or telephony>` | Shows the aircraft operator's card:<br>• Three-letter designator (3LD), telephony, company and country<br>• A U.S. special call sign: its agency and expiration date instead<br>• A virtual airline your facility added: marked `--VA--`, with its virtual organization<br>*When entering the telephony, leave out spaces and special characters.*<br>*When several operators match, the card lists each of them.* | `.idDAL`<br>`.idDELTA`<br>`.idNASA` |

## Data Display

Draws an airway's or a procedure's fixes on your scope. Your facility may include only the airways and procedures in its own area.

| Syntax | Description | Example |
| --- | --- | --- |
| `.<airway ID>`<br>`f` | Shows every fix on the airway, NAVAIDs and airports included.<br>*CRC STARS & ERAM.* | `.J60F` |
| `.<airport ID>`<br>`<departure>`<br>`f` | Shows every fix on the departure procedure (a SID or an obstacle departure), NAVAIDs included, with all of its transitions.<br>*CRC STARS & ERAM.* | `.dtwCLVINf` |
| `.<airport ID>`<br>`<arrival>`<br>`f` | Shows every fix on the arrival procedure (STAR), NAVAIDs included, with all of its transitions.<br>*CRC STARS & ERAM.* | `.dtwGRAYTf` |

**Procedure Names**

- A departure name is the first part of its FAA computer code without the version number. For example: `ROG4.RZC` is `ROG`.
- An arrival name is the second part of its FAA computer code, without the version number. For example: `AALAN.BLAID2` is `BLAID`.
- A chart with no computer code is spelled out in full instead, without its version number, bracketed words, or the words RNAV, OBSTACLE and COPTER; spaces and punctuation are also removed. For example, `TURNAGAIN EIGHT` at ANC is `.ancTURNAGAINf`.

## Chart Recall

Opens an FAA chart in your web browser, straight from the FAA's d-TPP. Every airport the FAA publishes charts for is included. A command is a period, the airport's FAA ID, the chart's code, then `c`.

| Syntax | Description | Example |
| --- | --- | --- |
| `.<airport ID>`<br>`<approach type>`<br>`[variant]`<br>`<runway>`<br>`c` | An instrument approach. The approach type codes are below. | `.dtwI22Lc`<br>`.dtwLZ04Lc`<br>`.laxRY24Lc` |
| `.<airport ID>`<br>`v`<br>`<visual name>`<br>`<runway>`<br>`c` | A charted visual approach: a lower-case `v` (for "Visual"), then the approach's name with spaces and punctuation left out. | `.sfovQUIETBRIDGE28Rc`<br>`.mryvRACEWAY28Lc` |
| `.<airport ID>`<br>`<procedure>`<br>`c` | A departure, an obstacle departure or an arrival (STAR). | `.dtwCLVINc`<br>`.dtwGRAYTc`<br>`.ancTURNAGAINc` |
| `.<airport ID>`<br>`<chart>`<br>`c` | Another of the airport's charts, such as its airport diagram. The chart codes are below. | `.dtwAPDc`<br>`.laxHSc` |
| `.<airport ID>`<br>`<chart code>`<br>`c`<br>`[page]` | Page 2 or later of a chart with more than one page: the page number goes after the `c`. | `.dtwCLVINc2` |

### Approach type codes

FE-Buddy uses the eight approach types in common use across the FAA.<br>A `/DME` approach adds `D` to its type's code, while a back course approach adds `BC`.

| Approach | Code | Example |
| --- | --- | --- |
| RNAV (GPS), RNAV (RNP) | `R` | `.laxRZ07Rc` |
| ILS | `I` | `.dtwI22Lc` |
| LOC | `L` | `.dtwL22Lc` |
| VOR | `O` | `.cdbO15c` |
| NDB | `N` | `.iliN36c` |
| LDA | `D` | `.dcaD19c` |
| GPS | `G` | `.fotG11c` |
| TACAN | `T` | `.pdxT28Lc` |
| LOC/DME | `LD` | `.fulLD24c` |
| VOR/DME | `OD` | `.talOD07c` |
| NDB/DME | `ND` | `.adkND23c` |
| LDA/DME | `DD` | `.ekoDD24c` |
| LOC BC | `LBC` | `.cdbLBC33c` |
| LOC/DME BC | `LDBC` | `.griLDBC17c` |

### Reading an approach's command

- **One command per approach**
  - A chart for more than one approach has a command for each:
    - ILS OR LOC RWY 22L at DTW is both:
      - `.dtwI22Lc`
      - `.dtwL22Lc`
- **Variant letters** (X, Y, Z...)
  - Come after the type code.
  - If the FAA indicates the variant on only one of the approaches on the same chart, the command applies the variant to both:
    - ILS Z OR LOC RWY 04L is both:
      - `.dtwIZ04Lc`
      - `.dtwLZ04Lc`
- **Runways** are written exactly as the chart's name prints them
  - RNAV (RNP) Z RWY 07R at LAX is:
    - `.laxRZ07Rc`
  - A chart for two runways has a command for each
    - TIPP TOE VISUAL RWY 28L/R at SFO is both:
      - `.sfovTIPPTOE28Lc`
      - `.sfovTIPPTOE28Rc`
- **Circling approaches**
  - Keep their letter where the runway would be, for example VOR-A at PDX is:
    - `.pdxOAc`
- **RNAV**
  - `R` = RNAV, regardless of what the brackets say, (GPS) or (RNP)
  - Only a GPS approach with no "RNAV" in its name is `G`.

### Charted visual approaches

- The approach's name is spelled out in full, without the words VISUAL and RWY; spaces and punctuation are removed.
- Its runway comes right after the name.

| Airport | Chart | Command |
| --- | --- | --- |
| SFO | QUIET BRIDGE VISUAL RWY 28R | `.sfovQUIETBRIDGE28Rc` |
| MRY | RACEWAY VISUAL RWY 28L | `.mryvRACEWAY28Lc` |
| LGB | LA RIVER VISUAL RWY 12 | `.lgbvLARIVER12c` |

### Departures, obstacle departures and arrivals

- A departure name is the first part of its FAA computer code without the version number. For example: `ROG4.RZC` is `ROG`.
- An arrival name is the second part of its FAA computer code, without the version number. For example: `AALAN.BLAID2` is `BLAID`.
- A chart with no computer code is spelled out in full instead, without its version number, bracketed words, or the words RNAV, OBSTACLE and COPTER; spaces and punctuation are also removed. For example, `TURNAGAIN EIGHT` at ANC is `.ancTURNAGAINc`.

### Other charts

| Chart | Code | Example |
| --- | --- | --- |
| Airport diagram | `APD` | `.dtwAPDc` |
| Takeoff minimums | `TM` | `.dtwTMc` |
| Diverse vector area | `DVA` | `.laxDVAc` |
| Radar minimums | `RM` | `.hsvRMc` |
| Hot spots | `HS` | `.laxHSc` |
| LAHSO | `LAHSO` | `.burLAHSOc` |

Takeoff minimums, diverse vector areas and radar minimums open straight to the airport's own page of the FAA's shared document.

### Charts with no command

- High-altitude (HI-) and COPTER charts
- PRM approaches
- Category II and III approaches, and other special-authorization approaches
- CONVERGING approaches
- GLS approaches (the other approaches on the same chart still have a command)
- Numbered approaches, such as VOR-1
- Attention All Users pages (AAUP)
- Alternate minimums

---

*Page updated on 3 October 2026.*
