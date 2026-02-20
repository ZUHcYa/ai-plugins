# trips Folder Structure

Overview of how trip files are organized on disk.

---

## Path Structure

trips uses a dynamic folder hierarchy based on your configured `dataDir`:

```
<dataDir>/
└── <YYYY>/                          ← year folder
    └── <MM - YYYY>/                 ← month folder (e.g. "02 - 2026")
        └── <FirstName LastName>/    ← employee subfolder
            ├── YYYY-MM-DD_destination.md    ← trip files
            └── _report-YYYY-MM.md           ← monthly report
```

**Example with `dataDir: "O/5 Support Processes/53 Backoffice/Auslagen"`:**

```
O/5 Support Processes/53 Backoffice/Auslagen/
└── 2026/
    └── 02 - 2026/
        └── Friedrich Wagner/
            ├── 2026-02-03_berlin.md
            ├── 2026-02-10_munich.md
            ├── 2026-02-17_hamburg.md
            └── _report-2026-02.md
```

---

## Why This Structure

| Level | Reason |
|-------|--------|
| Year folder (`2026/`) | Easy archiving by year |
| Month folder (`02 - 2026/`) | Natural boundary for monthly reports and tax filing |
| Employee subfolder | Team use: multiple people can record to the same `dataDir` |
| Trip file per trip | One file per trip = easy editing and querying |

---

## File Naming

| Type | Pattern | Example |
|------|---------|---------|
| Trip file | `YYYY-MM-DD_<destination-kebab-case>.md` | `2026-02-03_berlin.md` |
| Monthly report (Markdown) | `_report-YYYY-MM.md` | `_report-2026-02.md` |
| Monthly report (PDF) | `_report-YYYY-MM.pdf` | `_report-2026-02.pdf` |

**Destination naming rules:**
- Kebab-case: `new-york` not `New York`
- No umlauts: `muenchen` not `münchen`
- Keep it short: city name only (no street, no full address)

---

## Trip File Format

```yaml
---
name: Client meeting Berlin
startDate: 2026-02-03
endDate: 2026-02-03
destination: Berlin
purpose: Q1 planning meeting with [Kunde]
startTime: 09:00
endTime: 18:00
distanceKm: 320
reimbursementEur: 96.00
created: 2026-02-03
---

# Client meeting Berlin

| Field | Value |
|-------|-------|
| Date | 2026-02-03 |
| Destination | Berlin |
| Purpose | Q1 planning meeting with [Kunde] |
| Distance | 320 km |
| Reimbursement | €96.00 |
```

**YAML frontmatter is the single source of truth.** The Markdown table is generated for human readability but is not authoritative.

---

## Required vs. Optional Fields

| Field | Required | Notes |
|-------|----------|-------|
| `name` | Yes | Trip description |
| `startDate` | Yes | YYYY-MM-DD |
| `endDate` | Yes | YYYY-MM-DD |
| `destination` | Yes | City name |
| `purpose` | Yes | Business purpose |
| `startTime` | No | HH:MM |
| `endTime` | No | HH:MM |
| `distanceKm` | No | If driving |
| `reimbursementEur` | No | Auto-calculated from distanceKm × 0.30 |
| `created` | Auto | Set on creation |

---

## Configuration

**Location:** `.trips/config.json`

```json
{
  "dataDir": "O/5 Support Processes/53 Backoffice/Auslagen",
  "company": "Acme GmbH"
}
```

Employee name is read from Claude's User Preferences (not stored in config).

---

## Team Use

Multiple team members can record to the same `dataDir`. Each person gets their own subfolder named `<FirstName LastName>`. This allows a shared organizational filing system where individual records are cleanly separated.

```
Auslagen/
└── 2026/
    └── 02 - 2026/
        ├── Friedrich Wagner/
        │   ├── 2026-02-03_berlin.md
        │   └── _report-2026-02.md
        └── Maria Mueller/
            ├── 2026-02-05_hamburg.md
            └── _report-2026-02.md
```

---

**Version:** 0.4.0
