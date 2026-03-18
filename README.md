# ⚛️ Sandbox Fizica 2D

**Simulator fizic 2D interactiv** — proiect de fizica realizat de **Cristian Todirenchi**.

Un singur fisier HTML (~3760 linii), fara dependinte externe, ruleaza direct in browser.

---

## Caracteristici

### Motor fizic — Sequential Impulse Solver
- **Coliziuni** — SAT (Separating Axis Theorem) pentru poligon-poligon, algoritm dedicat pentru sfera-poligon
- **Broadphase** cu AABB overlap, **narrowphase** cu detectie exacta
- **Velocity iterations** (8) + **position iterations** (4) pentru convergenta stabila
- **Restitutie zero** pe contacte — obiectele nu ricoseaza, se comporta natural
- **Corectie Baumgarte** pentru penetrare cu slop si factor progresiv

### Forte fizice
| Forta | Descriere |
|-------|-----------|
| **Greutate** | `F = m · g` — gravitatie configurabila (0–25 m/s²) |
| **Normala** | Calculata automat din impulsuri de contact |
| **Frecare** | Model Coulomb complet — statica (`μs = 1.4 · μk`) + kinetica |
| **Tensiune** | Forta in sfoara inextensibila cu damping |
| **Elastica** | Resort Hooke `F = -k · Δx` cu rigiditate configurabila |
| **Rezistenta aer** | Drag aerodinamic `F = -½ · Cd · ρ · A · v²` |

### Obiecte — 32 preseturi
- **Forme de baza** — Cub, Triunghi, Cerc
- **Blocuri** — Dreptunghi, Cutie mica/mare, Bara, Stalp, Placa, Greutate, Gheata, Cauciuc
- **Sfere** — Bila mare/mica, Bolovan, Balon, Minge fotbal/tenis, Roata cauciuc, Bila otel, Bila bowling
- **Rampe** — Rampa stanga `/`, Rampa dreapta `\`, Triunghi dreptunghic, Pana, Tobogan, Munte
- **Platforme** — Platforma, Pod, Perete, Turn, Zid gros

Fiecare obiect are: masa, coeficient de frecare (μ), dimensiuni, culoare, optiune de ancorare.

### Conectori
- **Sfoara** — inextensibila, transmite tensiune doar cand e intinsa
- **Resort** — legea lui Hooke cu rigiditate configurabila (k = 1–500)

### Grafice in timp real — 21 grafice per obiect
Categorii:
- **Cinematica** (8) — Vx(t), Vy(t), |V|(t), ax(t), ay(t), |a|(t), x(t), y(t)
- **Rotatie** (2) — ω(t), θ(t)
- **Forte** (5) — Greutate(t), Normala(t), Frecare(t), Fnet(t), Drag(t)
- **Energie** (3) — Ec(t), Ep(t), Em(t)
- **Relatii** (3) — v(a), Ec(v), Ff(N)

Click pe grafic → **fullscreen** cu grila detaliata si etichete pe axe.

### Teme vizuale (5)
| Tema | Descriere |
|------|-----------|
| 🔬 Laborator | Fundal inchis, stil tehnic |
| ☀️ Zi senina | Cer albastru, soare, nori |
| 🌅 Apus | Gradient violet-portocaliu |
| 🌙 Noapte | Cer instelat cu luna |
| 🏜️ Desert | Cer cald, dune de nisip |

### Scenarii predefinite
- **Pe Luna** — g = 1.62 m/s²
- **Gheata perfecta** — μ = 0.01 pe toate obiectele
- **Pe Jupiter** — g = 24.8 m/s²
- **Vid (fara aer)** — Cd = 0, cadere libera pura

### Alte functionalitati
- **Rigla** pe axele Ox si Oy la origine (scale in metri)
- **Simulare** pana la 10 minute, viteza 0.1x – 10x
- **Gizmo-uri** vizuale pentru rotatie si unghi rampa (stil Unity/Unreal)
- **Slider unghi rampa** (α = 5°–85°) cu pastrarea ipotenuzei
- **Vectori de forta** pe fiecare obiect cu toggle etichete (afisare/ascundere valori numerice)
- **Camera** — zoom (0.2x–3x), pan cu click-dreapta, recentrare
- **Responsive** — se adapteaza la orice dimensiune de ecran
- **Undo/Redo** istoric de stari

---

## Cum rulez?

Deschide `index.html` in orice browser modern (Chrome, Firefox, Edge).

```
Nu necesita server, Node.js sau alte dependinte.
Un singur fisier — totul e inline (HTML + CSS + JS).
```

---

## Controale

| Actiune | Control |
|---------|---------|
| Play / Pause | `Space` sau butonul ▶ |
| Sterge obiect | `Delete` sau `Backspace` |
| Pan camera | Click-dreapta + drag |
| Zoom | Scroll sau slider |
| Selectare | Click pe obiect |
| Mutare obiect | Drag (cand simularea e oprita) |
| Rotire obiect | Drag pe cercul verde (gizmo) |
| Unghi rampa | Drag pe diamantul rosu (gizmo) |

---

## Structura codului

```
index.html (3760 linii)
├── CSS (~500 linii)
│   ├── Layout principal (banner, scene, panel, timeline)
│   ├── Componente UI (slidere, butoane, carduri)
│   ├── Grafice si legenda
│   └── Responsive (3 breakpoints)
│
└── JavaScript (~3200 linii)
    ├── Initializare canvas + polyfill
    ├── Culori si constante vizuale
    ├── Sistem de teme (5 teme cu decoratiuni)
    ├── Preseturi obiecte (32 forme)
    ├── Geometrie (vertecsi, AABB, centru, baza)
    ├── Forte conectori (sfoara + resort)
    ├── Motor fizic — Sequential Impulse Solver
    │   ├── Integrare semi-implicita Euler
    │   ├── Coliziuni sol + frecare
    │   ├── Detectie coliziuni SAT + sfera-poligon
    │   ├── Velocity iterations (impuls normal + frictiune)
    │   ├── Position iterations (corectie Baumgarte)
    │   └── Stabilizare rotatie pe suprafete
    ├── Sistem grafice (21 grafice + fullscreen)
    ├── Desenare vectori de forta
    ├── Desenare conectori (sfoara + resort)
    ├── Cadru principal (sky, grid, obiecte, gizmo-uri)
    ├── Selectie si management obiecte
    ├── Actualizare panou UI
    ├── Event handlers (slidere, mouse, tastatura)
    ├── Gizmo-uri vizuale (rotatie + unghi rampa)
    └── Bucla de animatie (game loop)
```

---

## Fizica implementata

### Integrare
Semi-implicit Euler: se actualizeaza **viteza** din forte, apoi **pozitia** din viteza actualizata.

### Coliziuni inter-obiecte
1. **Broadphase** — AABB (Axis-Aligned Bounding Box) overlap
2. **Narrowphase** — SAT pentru poligoane, algoritm punct-pe-segment pentru sfere
3. **Rezolvare** — impulsuri secventiale cu acumulare si clamp
4. **Frictiune Coulomb** — impuls tangential limitat de `μ · Jn`
5. **Corectie penetrare** — Baumgarte stabilization cu slop = 0.1px

### Suprimarea torque-ului artificial
Pe contacte cu obiecte ancorate (rampe, platforme), impulsul angular din forta normala este suprimat. Motivatie: un singur punct de contact genereaza torque artificial; in realitate, contactul distribuit nu produce torque net din normala.

### Stabilizare rotatie
Obiectele pe suprafete ancorate au un restoring torque care le aliniaza la unghiuri stabile (90° pentru blocuri, 120° pentru triunghiuri) cu damping puternic (0.85).

---

## Tehnologii

- **HTML5 Canvas** — desenare 2D
- **Vanilla JavaScript** — fara frameworks
- **CSS Variables** — sistem de teme
- **Google Fonts** — DM Sans + JetBrains Mono

---

*Proiect de fizica — Cristian Todirenchi*
