## 3) Core Extraction Checklist (No Code Changes)

Use this checklist **before** copying anything into the new repo:

### A) Identify “core engine” logic
- [ ] Map initialization (base layer, view, controls)
- [ ] Geometry helpers (point creation, buffers, unions)
- [ ] Area-weighted aggregation helpers
- [ ] Input parsing (CSV/GeoJSON upload utilities)
- [ ] Shared constants (buffer size defaults, CRS)

### B) Identify “module” logic
- [ ] Population/ACS logic
- [ ] Employment/LODES logic
- [ ] Community Risk (CRE) logic
- [ ] Essential Services logic
- [ ] LBAR Housing logic

### C) Identify UI shell pieces
- [ ] Sidebar layout (station controls, uploads, summaries)
- [ ] Map panel layout
- [ ] Module/metrics display panels

### D) Separate generic vs. project-specific
- [ ] Keep the **core engine** generic (no Small Starts labels or breakpoints)
- [ ] Move any scoring/rating thresholds into a profile file
- [ ] Move any copy text or labels into profile-driven UI config

### E) Migration plan (safe, staged)
- [ ] Copy only core utilities first
- [ ] Add 1–2 modules as proof of concept
- [ ] Add a single profile file that mirrors current behavior
- [ ] Validate results match the original tool before adding more

This staged approach prevents regressions and keeps the existing project safe.
