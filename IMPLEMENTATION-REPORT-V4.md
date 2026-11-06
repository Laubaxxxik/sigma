# Matchbook Awards V4 · implementation report

## Files

Added `awards-v4-engine.js`, `awards-v4-ui.js`, `awards-v4.css`, `assets/football-stadium.jpg`, `tests/awards-v4.test.cjs`, `tests/ui-flow.cjs`, and the V4 documentation.

Updated `index.html`, `awards-outlook-v3726.js`, `matchbook-awards-evidence-v3728.js`, `matchbook-trophy-v3730.js`, `matchbook-seasons-v3728.js`, `action-creator-v3719.js`, `quality.js`, `clubs.js`, `tracking.js`. Changed application scripts receive new cache-version URLs.

## Award models

All figures below are internal Matchbook calibration, not official voting. Component points are bounded, weighted and rounded; their sum is the internal score.

| Award | Components and maximum points |
|---|---|
| Ballon d'Or | Individual 40; Big Matches 20; collective 20; consistency 10; Europe/international 10 |
| FIFA The Best | Individual 40; Big Matches 20; collective 20; consistency 10; competition/international context 10 |
| Kopa | Individual 45; Big Matches 15; collective 15; consistency 10; breakthrough/competitive context 15 |
| Domestic Player of the Season | League performance 45; important league matches 25; consistency 15; league success 10; rankings 5 |
| UCL Player of the Season | UCL performance 45; important UCL matches 25; consistency 15; UCL success 10; rankings 5 |
| Playmaker of the Year | Creation 55; Big Matches 15; collective 10; consistency 10; competition context 10 |
| Gerd Müller | Recorded eligible goals 65; goals/90 25; goalscoring distinctions 10 |
| Yashin | Save percentage 35; clean-sheet frequency 20; ratings 20; Big Matches 15; collective 10 |
| Puskás | Technique 30; originality 20; preparation 20; placement 15; difficulty 15 |
| European Golden Shoe | Recorded league goals × individually confirmed season/competition coefficient, separate from 0–100 |

Position benchmarks distinguish forwards, wings, midfielders, defensive roles and goalkeepers. Ratings use a calibrated 6–9 conversion. Per-90 metrics use observed minutes; unobserved optional metrics are omitted, with coverage disclosed. Defensive roles use defensive benchmarks rather than striker goal targets. Configuration lives in `CONFIG` in the V4 engine.

Big-Match Influence evaluates performance in the shared classifier's matches and scales evidence to 450 recorded Big Match minutes. Merely playing a final does not produce a high performance component. Europe respects UCL/UEL/UECL strength (1/.8/.6). FIFA uses its own configured period and a distinct context blend; Kopa's final component also requires sustained playing time and competitive evidence.

Collective scoring uses confirmed campaign titles and actual player campaign minutes. Recorded progression receives a smaller bounded contribution and never becomes a trophy. Undated titles are excluded from explicitly date-bounded award periods. Kopa eligibility needs a sourced edition age limit, reference date and birth date; no current-day age shortcut. Yashin is restricted to goalkeepers and requires complete recorded saves/conceded data.

Gerd Müller's 50-goal and 1.0-goal/90 normalization references are internal benchmarks only; edition-specific eligible categories and dates are configurable. No goal threshold awards victory. Its qualitative labels describe goalscoring distinction rather than a world placement.

## Thresholds and TOP 10

Broad default bands: 0–39 not in contention; 40–54 radar; 55–69 emerging; 70–79 serious; 80–89 potential TOP 10; 90–100 potential favourite. Broad thresholds can be edited for each award in Edition rules.

TOP 10 requires score ≥80, Individual ≥70, at least two positive dimensions, complete recorded minutes ≥1,500, rating coverage ≥60%, identifiable competition context ≥80%, and at least 75% match-level coverage for a selected-season analysis. Favourite additionally requires score ≥90, Individual ≥85 and strong Big Match/context/collective evidence. These gates are inspected in Analysis and are independent of confirmed placements.

Under 540 minutes: insufficient evidence. 540–899: provisional. 900–1,499: developing. Missing minutes or inadequate coverage blocks high-confidence tiers. Rankings and scoring cannot manufacture an official shortlist position.

## Competition rankings and double-counting

Positions are stored independently by season and actual competition ID/name. Domestic cups are not merged; UCL, UEL and UECL remain distinct. Recorded goals/assists beside each position come from that competition's canonical match rows; aggregate imported history is not assigned to competitions.

Distinction bases for 1st / 2nd–3rd / 4th–10th: UCL 10/7/4; major league 8/6/3; UEL 7/5/2; UECL 5/3/1; cup 3/2/1. Assists multiplier .8. Current positions receive .6 of final-position evidence; recorded competition minutes scale to a 900-minute sample. Other leagues require a versioned strength coefficient.

The combined distinction is capped at 100 normalized points. It replaces 20% of the Individual component, yielding at most 8 final Ballon/FIFA points or 9 Kopa points. Domestic/UCL awards use only their explicit 5-point ranking component. Creation awards use assists distinctions; Gerd Müller uses goalscoring distinctions. No ranking points are added outside an award's 100-point budget. Raw statistics, team contribution and match context have explicit component roles, rather than independent uncapped action bonuses.

## Puskás

Only attributable recorded Goal/Penalty goal events are analysed. Contradictory saved outcomes, disallowed/own goals, derived duplicates, repeated event identities, known fair-play exclusions and out-of-period goals are excluded. Historical events lacking identifiers are displayed with a limitation and stable match-index identity; no raw ID is rewritten.

Technique, recorded preparation, body part, pressure, shot origin, goal-mouth placement and actual linked successful dribbles provide the evidence. A recorded ordinary volley receives less than an explicitly unusual technique. A corner placement provides at most its 15-point component. Linked dribbles must precede the goal in the same saved sequence and belong to the scorer. There is no invented dribble chain, opponent error, ball curvature or trajectory.

Unknown components remain unknown. The displayed score is the sum of supported evidence, with coverage and missing attributes disclosed. Labels: <40 ordinary; 40–59 notable; 60–74 impressive; 75–89 exceptional; 90+ outstanding. Incomplete data receives an explicit limited-assessment label rather than being called an ordinary finish. Original match, minute, attributes, component scores and the existing shot/goal-mouth markers are available in the goal viewer.

## Results and historical data

V4 results store confirmation method (official/manual or simulated), year, nomination, placement, winner, votes and source separately. They appear in award cards and career history. Legacy outcomes remain labelled legacy ceremonies; no old result is silently promoted to official confirmation. Existing ceremony/inbox ownership remains unchanged.

Historical analysis is recomputed on demand for selected seasons, including saved seasons covered by explicitly configured eligibility windows. There is no destructive migration or write-back of derived scores. Repeated calculation is idempotent. Locked snapshots remain intact; new ranking edits never edit historical match totals. The supplied archive contained no actual saved career database, so no user's real historical results were recalculated or modified during this work.

## Confirmed defects fixed

1. **Saved-career reload failure:** historical shot rebuilding called `matchbookShooterFoot` while `app.js`'s lexical `db` was uninitialized. Even `typeof db` threw. The lookup now safely handles that initialization interval; browser save/reload and backup restoration passed.
2. **Rendering bottleneck:** stadium resolution repeatedly scanned and normalized the full club catalogue. Added an incremental alias index preserving first-match semantics and extension-appended clubs.
3. **Missing metrics treated as zero:** partial event reports previously supplied zero optional statistics to award analysis. The canonical report now respects `_observed`, positive recorded observations and explicit full tracking.
4. **Trophy Room content offset:** global `main` styles affected the nested Trophy Room main. Scoped layout overrides remove the inherited sidebar offset.
5. **Historical season navigation:** Your Seasons previously updated the old Trophy Room selection only. It now also updates the active V37.30/V4 room.
6. **Evidence scope:** domestic and UCL award evidence previously displayed all-season totals. Evidence now uses each assessment's eligible rows.
7. **Ranking precision:** ranking distinction points are rounded consistently to avoid floating-point display discrepancies.

## Validation and audit boundaries

Executed: pure engine/data tests, the app's original regression checks, browser workflows, desktop/mobile visual inspection, JavaScript syntax checks, and local asset-reference/ZIP integrity checks. Exact totals are recorded in `VALIDATION-RESULTS.txt`.

Browser workflows cover both galleries, evidence, Puskás markers, rankings/results save and reload, Your Seasons, historical season navigation, match editing, Inbox, ceremony rendering, and actual JSON download/import restoration. New fields are validated through the existing import/load validator. No uncaught browser errors were observed in the final workflow.

This was a targeted codebase audit with syntax coverage across all shipped JavaScript and functional checks of the relevant integrations; it is not a claim that every possible historical save, every action type or every screen state was exhaustively tested.

## Remaining limitations

- Edition rules, coefficients and official placements are not bundled as an independently verified worldwide dataset. Enter the applicable sourced edition metadata. Unverified information stays visibly unverified.
- Date-specific scoring excludes undated collective achievements. Comparative world leaderboards and official votes are never inferred.
- Goalkeeper assessment requires reliable saved goalkeeper data; saves alone cannot reconstruct every match's coverage.
- Historical aggregate-only totals cannot produce reliable per-competition/per-90 or detailed goal evidence.
- Cloud library retrieval was unavailable in this environment. Local storage, IndexedDB-backed reload and JSON backup/import were tested; live cross-device cloud synchronization and an actual Vercel deployment were not tested or performed.
- No earlier visual-reference image was included in the supplied files. The design follows the requested ivory/plum palette and two large photographic organization cards, with existing trophy illustrations.

Photographic background: Unsplash image `photo-1522778119026-d647f0596c20`, downloaded as a bundled asset. Organization names identify galleries; the UI does not claim organizational endorsement.
