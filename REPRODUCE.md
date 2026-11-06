# Reproducing this repository

```
python tools/fetch_cap.py <cache> us f f2d f3d f-appx
python tools/build.py supreme-court <cache>/<reporter>,... <empty folder> --today 2026-11-06
```

Input: static.case.law reporter volume zips (CC0). Gives 328,247 decision commits and one documentation commit.
