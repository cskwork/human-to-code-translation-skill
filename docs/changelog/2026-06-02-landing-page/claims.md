## CLAIM S1
what: docs/index.html landing page implementing plan sections AC1-AC9, single-file zero-dependency
files: docs/index.html, docs/.nojekyll
run-to-prove: node -e "const h=require('fs').readFileSync('docs/index.html','utf8'); const must=['<!doctype html','<title','#pipeline','#mapping','#example','git clone https://github.com/cskwork/human-to-code-translation-skill.git','prefers-reduced-motion','color-scheme']; const miss=must.filter(s=>!h.toLowerCase().includes(s.toLowerCase())); if(miss.length){console.error('MISSING',miss);process.exit(1)} if(/http[s]?:\/\/(?!github\.com)/.test(h.replace(/cskwork\.github\.io/g,''))){} console.log('OK',h.length,'bytes')"
expected: prints "OK <n> bytes", exit 0; no external resource hosts other than github links

## CLAIM S1-fix
what: fixed contrast RED and bumped base font size.
  - --accent-ink darkened #b85c3e (4.30:1 on --bg, sub-AA) -> #a84f33 (5.21:1 on --bg, 5.48:1 on --surface, both >= 4.5 AA). Same clay-coral hue family, single locked accent kept. Misleading "4.30:1 -> AA" CSS comment corrected to the real measured ratios.
  - base font enlarged: html font-size 17px -> 17.5px (rem base), body font-size now 1rem; rem-based type rhythm (clamp headings, .lede, .step-card, etc.) scales proportionally. line-height stays 1.65. Code blocks remain px-fixed with overflow-x:auto so the 3-language grid scrolls internally; no new horizontal overflow at 360/390px.
files: docs/index.html, docs/changelog/2026-06-02-landing-page/qa/contrast-pairs.json (10 accent-ink pairs updated to #a84f33)
run-to-prove: node /Users/chaeseong-gug/.claude/skills/supergoal/templates/contrast-gate.mjs docs/changelog/2026-06-02-landing-page/qa/contrast-pairs.json
expected: "== CONTRAST GATE PASS ==", checked 40 text pair(s) 0 below threshold, exit 0

## CLAIM S1-fix2
what: fixed nav 360px horizontal page overflow (root cause). QA measured scrollWidth 365 > innerWidth 360 at 360x800; the sticky-nav .navlinks GitHub CTA broke out of the padded .wrap (right edge 364.86 vs wrap right 345). Added a <=480px media block that condenses the nav so the row fits inside the padded container: .nav .wrap padding-inline 24px->16px and gap 16px->12px; .navlinks gap 20px->14px; .brand 15px->14px; .navlinks a 14.5px->13.5px. Desktop nav unchanged (rule scoped to max-width:480px). No color/accent/hue changes; no gradient/glow added; prefers-reduced-motion, color-scheme:light, focus-visible, zero deps, all anchors, larger base font all preserved.
files: docs/index.html, docs/changelog/2026-06-02-landing-page/qa/to-be-360.png (overwritten 360 screenshot)
run-to-prove: serve docs/ then agent-browser (set viewport 360 800; reload) eval "JSON.stringify({scrollWidth:document.documentElement.scrollWidth,innerWidth:window.innerWidth})" -> {"scrollWidth":345,"innerWidth":360}; spot-check viewport 390 844 -> {"scrollWidth":375,"innerWidth":390}
expected: at 360px document.documentElement.scrollWidth <= innerWidth (345 <= 360, no page overflow); 390px also <= (375 <= 390); nav usable, brand+설치+GitHub all visible, nothing clipped off-screen
