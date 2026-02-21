# Hook Strength Calculator — Winkler-Bach Formula

An interactive single-page web app for analyzing stress in curved beams (crane hooks) using the Winkler-Bach theory. Adjust parameters via sliders and see real-time stress distribution, cross-section geometry, and key results.

## Quick Start

Open `index.html` in any modern browser. No build step, no dependencies.

## Theory

### Winkler-Bach Stress Equation

The tangential (circumferential) stress at radial position `r` in a curved beam:

```
sigma(r) = M * (R - r) / (A * e * r)
```

For a hook under load F, the total stress at the critical cross-section includes both bending and direct axial components:

```
sigma_total(r) = F/A + M * (R - r) / (A * e * r)
```

Where:
| Symbol | Description | Unit |
|--------|-------------|------|
| `F` | Applied force | N |
| `M` | Bending moment at critical section = -F * Rc (negative: straightening) | N*mm |
| `A` | Cross-sectional area | mm^2 |
| `Rc` | Centroidal axis radius (distance from center of curvature to centroid) | mm |
| `R` | Neutral axis radius = A / integral(dA/r) | mm |
| `e` | Eccentricity = Rc - R (always positive, neutral axis is shifted inward) | mm |
| `r` | Radial position of the point of interest | mm |
| `ri` | Inner radius (intrados) | mm |
| `ro` | Outer radius (extrados) | mm |

### Key Stresses

- **Inner fiber** (intrados, r = ri): Compressive bending stress (partially offset by direct tensile F/A)
- **Outer fiber** (extrados, r = ro): Tensile stress (bending + direct axial both tensile)
- The stress distribution is **hyperbolic**, not linear as in straight beams

### Neutral Axis Radius by Cross-Section

**Rectangular** (width `b`, radial height `h = ro - ri`):
```
R = h / ln(ro / ri)
```

**Trapezoidal** (inner width `b1`, outer width `b2`, height `h = ro - ri`):
```
R = A / [(b2 - b1) + ((b1*ro - b2*ri) / h) * ln(ro/ri)]
where A = h * (b1 + b2) / 2
```

**Circular** (diameter `d`, centered at `Rc = (ri + ro) / 2`):
```
R = A / [2 * pi * (Rc - sqrt(Rc^2 - (d/2)^2))]
where A = pi * (d/2)^2
```

## Features

### Interactive Sliders

| Parameter | Range | Default | Unit |
|-----------|-------|---------|------|
| Force F | 1 - 100 | 10 | N |
| Inner radius ri | 10 - 100 | 30 | mm |
| Outer radius ro | 30 - 200 | 80 | mm |
| Width b (rectangular) | 5 - 100 | 40 | mm |
| Inner width b1 (trapezoidal) | 5 - 100 | 50 | mm |
| Outer width b2 (trapezoidal) | 5 - 100 | 30 | mm |
| Diameter d (circular) | 10 - 150 | 50 | mm |

### Supported Cross-Sections

1. **Rectangular** — simplest case, uniform width
2. **Trapezoidal** — most common for crane hooks (per DIN 15401), wider at inner radius for strength
3. **Circular** — solid round cross-section

### Visualizations

All update in real time as sliders are adjusted:

- **Cross-Section Diagram** — shows the selected shape with labeled dimensions, neutral axis (green dashed), and centroidal axis (yellow dashed)
- **Hook Side View** — semicircular hook profile showing inner/outer radii, applied force, and critical section location
- **Stress Distribution Plot** — sigma vs r curve with:
  - Tension zone (red shading, positive stress)
  - Compression zone (blue shading, negative stress)
  - Neutral axis and centroidal axis marked
  - Inner/outer stress values labeled at the curve endpoints

### Results Panel

Displays computed values:
- Bending moment M
- Cross-sectional area A
- Centroidal radius Rc
- Neutral axis radius R
- Eccentricity e
- Inner fiber stress sigma_i (compression)
- Outer fiber stress sigma_o (tension)

## Sign Convention

- **Positive stress** = tension
- **Negative stress** = compression
- The bending moment is positive when it increases curvature. At the critical section of a hook loaded downward, M = -F * Rc (the load straightens the hook, decreasing curvature)
- At the critical section: inner fiber (intrados) is in **compression**, outer fiber (extrados) is in **tension**
- The direct axial stress F/A is uniformly **tensile** across the section and is superimposed on the bending stress

## Validation Notes

The neutral axis always lies between the centroidal axis and the inner radius (R < Rc). This is a fundamental property of curved beam theory — the shift increases with the curvature (smaller ri/ro ratio). For beams where ri > 5 * (ro - ri), the straight-beam approximation becomes acceptable.

## File Structure

```
HookStrengthCalc/
  index.html    -- Complete app (HTML + CSS + JS, no dependencies)
  README.md     -- This documentation
```

## References

- Winkler, E. (1867). *Die Lehre von der Elasticitaet und Festigkeit.*
- Bach, C. (1889). Extension and experimental validation of Winkler's curved beam theory.
- Tripathi, Y. & Joshi, U.K. (2013). "Comparison of Stress Between Winkler-Bach Theory and ANSYS FEM for Crane Hook with a Trapezoidal Cross-Section." IJRET, Vol. 2, Issue 9.
- Boresi, A.P. & Schmidt, R.J. *Advanced Mechanics of Materials*, 6th ed. — Section 9.2: Curved Beams.

## License

MIT
