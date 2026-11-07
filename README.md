# Black-hole

A real-time, physically based simulation of a non-rotating (Schwarzschild) black hole, running on the GPU in any modern browser.

**Run it:** open `index.html` in a browser. It is a single file with no build step and no dependencies, and it needs WebGL 2. You can also enable GitHub Pages for the repository.

Drag to orbit, scroll or pinch to zoom.

## The physics

Every pixel is a light ray traced *backwards* from the camera along a null geodesic of the Schwarzschild metric. Units are geometric with the Schwarzschild radius `rₛ = 2GM/c² = 1`.

| Effect | How it is modelled |
|---|---|
| **Light bending / lensing** | The spatial path of a photon obeys exactly `d²r⃗/dλ² = −(3/2) rₛ h² r⃗ / r⁵` with `h = |r⃗ × ṙ⃗|` conserved. This is equivalent to the GR orbit equation `u'' + u = 3GMu²/c²`. It is integrated with 4th-order Runge–Kutta and an adaptive step. |
| **Event horizon & shadow** | Rays that reach `r = rₛ` are absorbed. Every ray with impact parameter `b < 3√3/2 rₛ ≈ 2.598 rₛ` is captured, so the shadow is 2.6× the horizon size. |
| **Photon ring** | Rays that wind around the photon sphere (`r = 1.5 rₛ`) produce the thin bright ring and the higher-order disk images. |
| **Accretion disk** | A thin Keplerian disk from the ISCO (`3 rₛ`) outward, with the Novikov–Thorne / Shakura–Sunyaev temperature profile `T ∝ r^-3/4 (1 − √(r_in/r))^1/4` and blackbody colours. |
| **Doppler shift, beaming and gravitational redshift** | Exact redshift for circular geodesic orbits: `g = √(1 − 3M/r) / (1 − Ωλ)`, with `Ω = √(M/r³)` and `λ = L_z/E` of the photon. Observed intensity scales as `g⁴` because `I_ν/ν³` is invariant, and observed colour temperature as `g·T`. |
| **Observer** | A static observer, including the gravitational blueshift of incoming light at the camera's radius. |
| **Stars** | Point sources drawn through the lens map's Jacobian, so lensed stars stay points and brighten by the true magnification. |

The side panel turns a chosen mass (stellar, Sgr A\*, M87\*, …) into real numbers: horizon size, ISCO orbital period, tidal acceleration, Hawking temperature, evaporation time, disk temperature at 10 % Eddington, and the camera's gravitational time dilation.

## Verification

The ray equation was checked numerically against known GR results:

- Critical (capture) impact parameter: numeric `2.59808 rₛ` vs exact `3√3/2 = 2.59808 rₛ`
- Weak-field deflection at `b = 200 rₛ`: numeric `1.0074×10⁻²` rad vs Einstein's `2rₛ/b + 15πrₛ²/16b² = 1.0074×10⁻²` rad

Each effect can be switched on and off to see what it contributes. The coordinate-grid sky makes the lensing distortion easiest to see.
