# CN-3064 dynamic form discovery — public test fixtures

Six self-contained static HTML pages for end-to-end testing of the
`central-scanner-worker` CN-3064 dynamic form discovery feature.

Served publicly via GitHub Pages at:
**https://revathysv.github.io/form-scanner-dynamic-test/**

## Fixtures

| Path | Exercises | Expected before CN-3064 | Expected after CN-3064 |
|---|---|---|---|
| `direct.html` | Regression guard | 1 form, no `discoveryMethod` field | 1 form, `discoveryMethod: "direct"` |
| `shadow.html` | Shadow DOM walker | 0 forms (invisible to `querySelectorAll`) | 1 form, `discoveryMethod: "shadow_dom"` |
| `accordion.html` | Accordion reveal pass | 0 forms (inside collapsed `<details>`) | 1 form, `discoveryMethod: "accordion_reveal"` |
| `tabs.html` | Tab reveal pass | Hidden panel filtered out | 1 form, `discoveryMethod: "tab_reveal"` |
| `spa.html` | SPA route listener | Soft nav missed by `waitForNavigation` | 1 form, `discoveryMethod: "spa_route"` |
| `wizard.html` | Multi-step chase | 1 form (step 1 only) | 3 forms, steps 2–3 tagged `multi_step_child` with `stepIndex` |

## Usage

Scan each URL from Automate on both the `main` worker branch and the
`CN-3064_dynamicForms` branch. Compare NDJSON output:

```bash
jq '.forms | length' form-scan-report.json
jq '.forms[].discoveryMethod' form-scan-report.json | sort | uniq -c
```

Related:
- Plan: CN-3064
- Contract: `central-scanner-worker docs/contracts/dynamic-form-discovery-v1.md`
- Worker PR: browserstack/central-scanner-worker#701
- Rails PR: browserstack/central-scanner#1907
- FE PR: browserstack/frontend#39154
