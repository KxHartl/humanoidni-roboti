# Pick-and-Place Zadaća 3

Repozitorij za interaktivnu Pick-and-Place CLI aplikaciju za branje voća s UR5e robotom i RealSense RGB-D kamerom.

## Što projekt radi

- Snima scenu iz više pozicija kamere.
- Pokreće YOLO detekciju i lokalizaciju voća.
- Rekonstruira point cloud i računa 3D centroide.
- Planira pick i place trajektoriju.
- Šalje izvršenje prema UR robotu.

## Struktura

| Putanja | Svrha |
|---|---|
| `src/main.py` | Glavni ulaz u pipeline |
| `src/adapters/`, `src/core/`, `src/usecases/` | Glavna logika pipelinea |
| `scripts/` | Skripte za kalibraciju, testiranje i treniranje YOLO modela |
| `data/camera_calibration/` | Kalibracijske matrice i transformacije |
| `data/datasets/` | Roboflow COCO skup podataka. `yolo_seg_dataset/` nije u repozitoriju, generira ga `scripts/yolo_training/prepare_dataset.py` |
| `data/models/` | YOLO model (`custom_fruit_seg.pt`), nije u repozitoriju |
| `data/raw/` | Snimljeni i generirani podaci |

## Pokretanje

Aktiviraj virtualno okruženje i pokreni glavni pipeline:

```powershell
.venv\Scripts\activate
python -X utf8 src\main.py
```

Za offline rad bez robota i kamere:

```powershell
python -X utf8 src\main.py --mock
```

Za rad s već snimljenim podacima:

```powershell
python -X utf8 src\main.py --offline --existing-captures data\raw\outputs\run_XXXXX\captures
```

## Ovisnosti

Osnovne Python ovisnosti su navedene u [requirements.txt](requirements.txt).

## Dokumentacija

- [STATE.md](STATE.md) - trenutno stanje projekta
- [task.md](task.md) - sažetak zadataka
- [TO_SHOW.md](TO_SHOW.md) - što se predaje

## Napomena

Projekt je reorganiziran tako da su podaci, modeli i kalibracije odvojeni od izvornog koda i smješteni u odgovarajuće direktorije unutar repozitorija.
