# V37.32 · Three premium galleries and realistic award presentation

## Scope and data safety

This is a presentation and navigation update over V37.31. `awards-v4-engine.js` is byte-for-byte unchanged (SHA-256 `f6d1afb230c533b76a6a1b7d264a8fa35c92cea51013fb2c0d7db6ce9f6cf99c`). No scoring weights, thresholds, eligibility rules or source-statistics calculations changed. There is no new career schema migration, storage key or trophy-generation path.

The browser test compares saved results, legacy nominations, full normalized match/event records and assessment outputs before and after navigation, season changes and reload. They remain unchanged. Existing load normalization still runs on reload, as it did in V37.31; tests begin with a normally prepared career fixture.

## Files changed

- `awards-v4-ui.js`: dedicated collection/award/goal views; result presentation; breadcrumbs; navigation history; selected-season player context. Existing ranking, result and rule editing flows retained. Existing result editor now also exposes Shortlisted and Finalist without changing old records.
- `awards-galleries-v4.css` (new): responsive editorial layouts, three equally prominent collections, featured awards, goal cards and dedicated details.
- `matchbook-trophy-v3730.js`: selected-season identity, cleaner primary tabs, removal of duplicate analytical award summaries from the ceremony page, neutral footer. Existing ceremony extraction and controls retained.
- `matchbook-awards-evidence-v3728.js`: secondary view explicitly titled Matchbook Performance Assessment; Golden Shoe's own points correctly labelled separately. Calculation source unchanged.
- `index.html`: new stylesheet and updated cache versions.
- `assets/ballon-dor-trophy.png`, `assets/fifa-best-trophy.png`, `assets/independent-honours.png` (new): original photographic-style trophy illustrations, visually inspected. They are not official event photographs and imply no endorsement or player achievement.
- `tests/ui-flow.cjs`: browser integration coverage adapted to the new information architecture.
- New README, report and validation transcript.

## Information architecture

Trophy Room → France Football / FIFA / Other Honours → Award → Puskás goal where applicable.

The homepage contains only the three collection cards and concise introductory copy. Selecting a collection replaces the homepage with its own view rather than expanding content in place. The existing Matchbook `page`/`managementView` architecture renders the view; nested state uses browser history, without writing to the career database. Browser Back/Forward, explicit breadcrumbs and reload preserve the selected season and relevant award/goal.

Wide desktop uses three columns. Laptop widths use two cards followed by a full-width third collection. Narrow screens stack all three. Dedicated pages keep the season selector, career context and existing app navigation.

## Award presentation

Collection cards emphasize the award's identity, edition, actual player and recorded result. Unconfirmed awards say “Season under evaluation,” rather than displaying a prediction as a nomination. Confirmed winners have a separate gold treatment; confirmed placements and nominations retain their meanings. Explicitly simulated outcomes stay labelled “Simulated career result.” Older records without official provenance stay labelled “Recorded career result” and are not silently promoted to confirmed official results.

Historical confirmed records are displayed independently, with year, club and saved source. A nominated player, a placed player and a winner are distinct. No competitors, nominations, votes, rankings or trophies are synthesized.

France Football normally features Ballon d'Or; FIFA normally features The Best. The featured item can change for relevant eligibility/evidence or a confirmed win. Other Honours prefers an applicable award with recorded season evidence; European Golden Shoe is not forced as the feature when its verified coefficient/coverage is unavailable and a domestic award is more relevant. It remains available in the European section. Goalkeeper-only Yashin is hidden for outfield players. Domestic and UCL awards use the player's actual competition context.

## Optional analysis and preserved workflows

One Award Insights interaction on each detail page opens the existing evidence view. Internal scores, gates, component weights, thresholds, missing data and statistical evidence remain there. They are absent from primary gallery cards. Golden Shoe league-goal points retain their own statistical identity.

The existing ranking editor remains under Your Seasons. Result confirmation and edition-rule editing remain under Manage award record in the award detail. Ceremonies retain their existing handlers and storage ownership. No FIFPRO World XI entry was added.

## Puskás and Other Honours

Puskás has a dedicated award detail with attractive saved-goal cards. Opponent, competition, date, minute, saved technique, explicitly saved distance and optional description are shown where available. A Matchbook candidate is explicitly separate from a FIFA nomination. Each goal opens its own saved record and reuses the original Shot Map/Goal Mouth marker functions. Unknown coordinates remain unknown. A closed Award Insights section contains the goal-quality subtotal, components and limitations.

Other Honours separates European, domestic and independent individual awards. A dedicated competition-distinction section summarizes independently recorded scorer/assist positions. It explicitly treats them as leaderboard distinctions, never trophies. Current and confirmed-final standings remain distinct. No unsupported domestic top-scorer trophy was invented.

## Verification executed

- 47 existing V4 engine/data regression tests passed.
- 22 original in-app regression checks passed.
- 21 browser scenarios passed, including all three collection routes, award details, correct saved Puskás goals, explicit and browser back-navigation, forward-navigation, season context, reload persistence, analysis, source records and scores unchanged, simulated-result separation, ranking save, match editor, ceremonies, Inbox and real JSON export/import.
- Responsive screenshots inspected at 1440px desktop, 1100px laptop and 390px mobile, including an award detail. Browser checks verify no horizontal document overflow and that all three bundled trophy images load.
- JavaScript syntax and local asset-reference checks, plus archive integrity and download verification, are recorded in the validation transcript.

No new uncaught browser runtime errors were observed.

## Limits

The generated trophy artwork is photographic-style illustration, not licensed official trophy photography. No supplied career database was available; browser data is explicitly test fixture data. Existing edition-rule verification, incomplete historical statistics and missing source attributes keep their V37.31 limitations. Live cross-device synchronization and production Vercel deployment were not performed; this environment's existing cloud-library retrieval remains unavailable. These limitations are not hidden by the cleaner gallery presentation.
