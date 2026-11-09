# Reproducing this repository

```
python tools/fetch_cap.py <cache> us f f2d f3d f-appx
python tools/build.py supreme-court <cache>/<reporter>,... <empty folder> --today 2026-11-06
```

Input: static.case.law reporter volume zips (CC0). Gives 328,247 decision commits and one documentation commit.

## The fixes of 2026-11-09
After the build and its appends, `tools/cap_fix_forward.py` brought this repo in line with the fixed builder, as new
commits on main (nothing rewritten). Reproduce it from the same inputs:

```
python tools/cap_plan.py <cap-bulk>                     # the fixed router: plan.json and route.tsv
python tools/cap_route_extract.py <cap-bulk> <parsed>   # every volume whose marker its files do not match is extracted again
python tools/cap_route_extract.py <cap-bulk> <parsed> --verify
python tools/cap_fix_forward.py supreme-court --today 2026-11-09  # decisions added, files corrected, index and relations
```

`tools/build_state.py` is the fixed builder (text-checked merges, head-matter fallback, counts checked against the
extraction and plan.json, the append filter on commit day and text). A fresh build with it gives the same decisions
and files as this repo's main after the fixes, though not the same history (here the fixes are later commits).
