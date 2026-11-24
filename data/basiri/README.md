# Basiri VES dataset (validation set)

**Source:** Aizebeokhai, A. P., Ogungbade, O., & Oyeyemi, K. D. (2018).
*Geoelectrical resistivity data set for characterising crystalline basement
aquifers in Basiri, Ado-Ekiti, southwestern Nigeria.* Data in Brief, 19,
810–816. https://doi.org/10.1016/j.dib.2018.05.091

**License:** CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/).
Files in `ves/` are the authors' supplementary file `mmc3.zip`, unchanged,
downloaded from Europe PMC (PMC5997927) on 2026-11-24. The 2D ERI files
(`mmc2.zip`) are not included; they are out of scope.

**Survey:** 30 Schlumberger VES, ABEM Terrameter SAS 1000/4000, April 2016.
Interpreted by the authors with WinResist.

## Files

| File | What it is |
|---|---|
| `VES n.DAT` | Raw field data for station n |
| `VES n.RST` | WinResist result for station n |
| `VES 26A.RST` | A second WinResist result for station 26 (3 layers instead of 4) |

## `.DAT` layout

- Line 1 header: `AB/2<TAB>APP RES`
- Then one row per reading: AB/2 in metres, apparent resistivity in Ωm,
  tab-separated.
- CRLF line ends. The last line has no line end.
- **No MN/2 column.** Some AB/2 values appear twice (e.g. 4.2, 13, 55 m).
  These are the overlap readings taken when MN was moved out. They are not
  errors.
- 16–21 readings per station. AB/2 from 1 m up to 42–130 m.

## `.RST` layout (WinResist output)

```
Schlumberger Array
VES n
L                                  <- number of layers
 Thick+Resis  lowlim  uplim  sigma
<L-1 rows: layer thicknesses, m, top layer first>
<L rows:   layer resistivities, Ωm, top layer first>
 Raw Dist  Raw Res.  Smoothed dist  Smoothed Res.  variances
<one row per reading used in the fit>
```

- The first column of the parameter rows is the value. The other columns
  are bounds and weights.
- WinResist used fewer readings than the `.DAT` has: the first two
  (AB/2 = 1.0 and 1.3 m) are left out on most stations.
- Duplicate AB/2 readings are averaged in the "Smoothed" columns.

## Known gaps

- The paper's Table 2 describes up to four layers per station, but the
  `.RST` files have five-layer models for VES 3, 5, 6 and 29. Compare the
  `.RST` values with Table 2 before using either as "the published answer".
- The `.RST` files do not give the RMS misfit.
