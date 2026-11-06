# MATCHBOOK NOIR — development report

## 1. Visual redesign

V38.0 replaces the previous Paper/Coral application shell with Midnight Edition. The sidebar, top bar, global season context, home dashboard, career centre and Media hierarchy are rebuilt. Statistics uses a workbench layout with filters, summary metrics and on-demand panels; seasons use a chronological timeline; calendar gains month/list presentation. Match History/Maker retains its two-column workflow with a scrollable editor. Award galleries, award details and Puskás retain their full-page navigation and real imagery within the new palette. Recovery is also dark and shares the font foundation.

## 2. Design system

Background #0b0e16, navigation #101521, surface #171c28, raised surface #202635, cream #f7f3eb, plum #663b61 and burgundy accent #a95c83. Gold marks honours. Positive/negative status colors remain semantic. Shared form sizing, table styling, keyboard focus, radii, shadows and responsive breakpoints are centralized. Manrope is self-hosted; its OFL license is included. Reduced-motion preferences are respected. Representative normal-text tokens exceed 4.5:1 against the main surface; this is not an exhaustive WCAG certification.

## 3. Route coverage

All 21 existing routes are accessible and included in the six-width rendering audit: Dashboard, Matches, Calendar, Statistics, Career, Seasons, Club squad, International, Transfers, Awards, Inbox, Media, Records, Timeline, Availability, Opponents, Notebook, Brackets, Tracking Guide, Export, Data & Help. Primary routes use the sidebar, supporting routes use its expandable More tools group. Unknown registered routes are retained.

Shared styling also covers match setup/result forms, fixture editors, season details/rankings, award record dialogs, ceremonies, Puskás goal details, Media composer/accounts/profile, backup/import, search and recovery. These supporting states do not all have exhaustive end-to-end coverage; see section 10.

## 4. Shared components

Semantic token foundation; desktop sidebar/mobile drawer with Escape and focus management; breadcrumb/top bar; global season selector; real local-save indicator; Tools menu preserving existing controls; skip link; reusable panels, headings, action buttons, empty states and calendar presentation switch. SVG pitches retain their coordinate systems and accessible interaction handlers. Club imagery falls back to initials when the existing external crest service is unavailable.

## 5. Functional improvements

The global season selector and Trophy Room selection now remain aligned. Optional session view state restores the route, selected season, statistics context, calendar month/presentation, Media tab and expanded season. It does not become a second career store. Analytical panels are built on demand. Native details controls no longer trigger accidental navigation. Calendar list presentation makes small-screen fixture access clearer. Match action bars follow normal document flow so they do not cover fields.

## 6–7. Bugs discovered and fixed

- A legacy statistics exception removed the current theme class: disabled for Noir.
- A palette compiler initially classified text variables containing “paper” as backgrounds: text/ink mapping takes priority.
- Earlier presentation wrappers overwrote the new Career and Dashboard: bypassed for Noir.
- An action attribute on the body caught ordinary clicks: body state now uses a distinct attribute. Calendar presentation similarly uses a separate attribute from its buttons.
- Lazy statistics panels initially used the old visibility-only switching handler: switches now render and bind the active panel.
- Historical award ceremonies used the current profile and a hardcoded nationality: presentation now reads the nomination season profile and saved nationality.
- Stale award history state could hijack refresh after leaving Awards: cleared when navigating elsewhere.
- Calendar min-width and horizontal career moments clipped small screens: responsive grid/min-width fixes.
- Legacy Match Maker viewport heights, flex-zero sizing and sticky bars clipped fields/overlapped actions: changed to a document-flow editor with one dialog scroll area.

## 8. Data and migration

No career schema migration is introduced. No user matches, events, IDs, historical results, award outcomes or geometry are rewritten for the redesign. Existing migrations/persistence remain in place. The only new storage key is optional sessionStorage `matchbook-noir-view-v1`. Corrupt/unavailable optional view state is ignored. The original primary local save, restore points, IndexedDB mirror, JSON import/export and cloud implementation remain authoritative.

Byte comparisons against V37.32 confirm that app.js, engine.js, tracking.js, realism.js, architecture.js, quality.js, cloud.js, matchbook-persistence-v3729.js and awards-v4-engine.js are unchanged. V4 engine SHA-256: `f6d1afb230c533b76a6a1b7d264a8fa35c92cea51013fb2c0d7db6ce9f6cf99c`.

## 9. Tests executed

Actual transcripts are included in validation/. The delivered test scripts are repeatable. Checks use synthetic fixtures rather than a user's private career.

- 47 award engine/model tests.
- 21 gallery browser checks, including nested back/forward/reload, season-specific identities, Puskás geometry, confirmed/simulated separation, ranking edits and real JSON download/import.
- 22 existing in-app football regressions, including xG invariance, penalty xG, migration idempotence, two-leg ordering and shootout isolation from aggregate goals.
- 126 route/viewport renders: 21 routes at 1920, 1440, 1366, 1024, 768 and 390 pixels. Audits check horizontal overflow, offscreen content, bright surfaces and runtime errors.
- 12 new Noir workflow checks cover single navigation handling, native details, category filters, lazy panels, shot selection, season refresh, award context, season shortcut, fixture creation/edit/postponement, starting a fixture, adding/editing a goal once, preserving ID/geometry, refresh recovery and final-whistle save.
- 8 resilience checks cover Media account save/reload, mobile drawer/Escape, fixture forms and Match Maker at six widths, local quota failure retaining the last persisted career, reduced motion and representative token contrast.
- 3 extra-time/shootout UI checks are reported in the separate transcript, including period boundaries and keeping shootout kicks separate from match goals.
- Visual inspection of desktop Dashboard, Statistics, Seasons, Calendar, Media, Career and Match Maker, plus mobile Dashboard and supporting dialog screenshots. Syntax and stylesheet generation are checked before packaging.

## 10. Tests not executed

No production Vercel deployment, authenticated live cloud synchronization/cross-device conflict exercise, real user private-save audit, Safari/Firefox/device-lab pass, screen-reader audit, exhaustive accessibility certification, visual pixel-diff baseline, or exhaustive combinations of every international tournament, transfer, Media publication/comment thread, season closure and exceptional match rule. Existing deterministic tests cover selected football edge cases; they are not equivalent to exhaustive UI coverage.

## 11. Known limitations

The local static test host cannot serve Vercel API routes. The existing cloud-library and fixture-difficulty requests report unavailable services there; the application shows the actual failure and local saving remains usable. External club crest requests may fail, in which case existing initials fallbacks are used. Live cloud behavior depends on the original environment configuration and is not newly implemented or claimed verified. The compatibility CSS bundle is intentionally retained (~733 KB before HTTP compression); this protects legacy component behavior but leaves further selector cleanup possible. Large tables use contained horizontal scrolling. Mobile month cells are compact and fixture details remain available in the list.

## 12. Files modified and added

Modified source: index.html, recovery.html, statistics-workspace.js, v34-ui.js, v35-design.js, career-v2.js, matchbook-seasons-v3728.js, matchbook-trophy-v3730.js, awards-v3719.js. Changes in these files concern presentation, selected-season synchronization and active-panel rendering.

Added: noir-foundations.css, noir-legacy.css, noir.css, noir.js, scripts/compile-noir.cjs, scripts/noir-style-sources.json, optional scripts/package.json, licensed assets/fonts, Noir browser tests, this report, README and validation artifacts. Original source stylesheets are retained. The detailed comparison against V37.32 is in validation/source-changes.json.

## 13. Build status

Static application; no new required build step or runtime package dependency. The checked-in compatibility stylesheet is generated by the optional PostCSS tooling. Frontend script parsing and local HTTP browser verification are recorded. Archives exclude node_modules, credentials, browser storage and real user data.

## 14. Vercel considerations

Use the existing static application root and retain api/ and existing environment variables. No hosting architecture change is required. Upload/extract the complete source folder, including all assets and the generated CSS. Modified script/style URLs have V38.0 cache versions. A different preview URL has separate browser-local storage: import a JSON backup explicitly to test a career there. Keep the production origin for continuity of its local storage. This delivery is a dedicated GitHub branch and downloadable archive; production is not overwritten automatically.
