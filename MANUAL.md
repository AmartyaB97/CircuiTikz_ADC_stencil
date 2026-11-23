# `adc_stencils` User Manual

A Visio-style CircuiTikZ/TikZ stencil library for drawing publication-grade analog, mixed-signal, and ADC schematics in LaTeX.

---

## 1. Installation (One-Time Setup)

Clone the repository and symlink `adc_stencils.sty` into your local `~/texmf` tree so `\usepackage{adc_stencils}` works from any directory:

```bash
git clone https://github.com/AmartyaB97/adc-circuitikz-stencils.git
mkdir -p ~/texmf/tex/latex/adc_stencils
ln -sf "$(pwd)/CircuiTikz_ADC_stencil/adc_stencils.sty" ~/texmf/tex/latex/adc_stencils/adc_stencils.sty
```

---

## 2. Minimal LaTeX Preamble

```latex
\documentclass{article}
\usepackage[american]{circuitikz}
\usepackage{adc_stencils}

\begin{document}
\begin{tikzpicture}[>=Latex]
    % Your schematic here
\end{tikzpicture}
\end{document}
```

> **Note:** Do **not** pass the `siunitx` option to `circuitikz` if your TeX Live installation does not include `siunitx.sty` / `xstring.sty`.

---

## 3. Core Concepts

### Placing a Stencil (`\pic`)
Every stencil is a TikZ `pic` with an instance name `(myname)` and coordinates `(x, y)`:

```latex
\pic (apf) at (0, 0) {apf_block};
\pic (op)  at (4, 0) {diff_opamp={2-stage\\Opamp}};
```

### Connecting Pins Orthogonally
Every stencil exposes named pins as `(myname-pin)`:
- `--` : straight line
- `|-` : vertical first, then horizontal
- `-|` : horizontal first, then vertical

```latex
\draw[adc wire] (apf-outp) -- (op-inn);
\draw[adc wire] (apf-outn) -- (op-inp);
```

### Wire Styles
| Style | Usage |
| :--- | :--- |
| `adc wire` | Plain solid black wire (for all analog/circuit connections, passives, switches, opamps, clocks, transistors) |
| `adc arrow` | Solid wire with Latex arrowhead (for block-level/DSP dataflows) |
| `adc bus` | Thick (`1.4 pt`) digital bus line |
| `adc bus arrow` | Thick (`1.4 pt`) digital bus line with arrowhead |

---

## 4. Complete Stencil Reference

### Interconnect, Terminals, Pins, & Rails
| Stencil | Example | Pins |
| :--- | :--- | :--- |
| `terminal` | `\pic (t1) at (0,0) {terminal};` | `-center` |
| `ioport` | `\pic (p1) at (0,0) {ioport={$V_{\mathrm{in}+}$}};` | `-in`, `-out`, `-center` |
| `iopin` | `\pic (p2) at (0,0) {iopin};` | `-in`, `-out`, `-lead`, `-center` |
| `noconn` | `\pic (nc) at (0,0) {noconn};` | `-center` |
| `jumper` | `\pic (j1) at (0,0) {jumper};` | `-in`, `-out`, `-peak`, `-center` |
| `bus_tick` | `\pic at (0,0) {bus_tick={4b}};` | `-center` |
| `vdd` | `\pic (v1) at (0,0) {vdd={$V_{\mathrm{DD}}$}};` | `-pin`, `-center` |
| `gnd` | `\pic (g1) at (0,0) {gnd};` | `-pin`, `-center` |

### Passives, Switches, & Transistors
| Stencil | Example | Pins |
| :--- | :--- | :--- |
| `res` / `vres` | `\pic (r1) at (0,0) {res={$R$}};` | `-p`, `-n`, `-center` |
| `cap` / `vcap` | `\pic (c1) at (0,0) {cap={$C_{\mathrm{int}}$}};` | `-p`, `-n`, `-center` |
| `ind` | `\pic (l1) at (0,0) {ind={$L$}};` | `-p`, `-n`, `-center` |
| `sw_open` / `sw_close` | `\pic (s1) at (0,0) {sw_open={$\Phi_1$}};` | `-in`, `-out`, `-clk`, `-center` |
| `sw_2port` | `\pic (s2) at (0,0) {sw_2port={$\Phi_{\mathrm{sel}}$}};` | `-in`, `-out1`, `-out2`, `-clk` |
| `nmos` | `\pic (m1) at (0,0) {nmos={$M_1$}};` | `-g`, `-d`, `-s`, `-b`, `-center` |
| `pmos` | `\pic (m2) at (0,0) {pmos={$M_2$}};` | `-g`, `-s`, `-d`, `-b`, `-center` |

> **Tip for Mirrored Transistors:** Use `[xscale=-1]` with an empty label (`{pmos={}}`) and place the label manually on the left (`\node[left=8pt] at (m-center) {$M_3$};`) so the text isn't mirrored.

### Amplifiers, Converters, & MUX/DEMUX
| Stencil | Example | Pins |
| :--- | :--- | :--- |
| `opamp` | `\pic (a1) at (0,0) {opamp={$A_v$}};` | `-inn`, `-inp`, `-out` |
| `diff_opamp` | `\pic (op) at (0,0) {diff_opamp={2-stage\\Opamp}};` | `-inn`, `-inp`, `-outp`, `-outn`, `-cmfb` |
| `comparator` | `\pic (cmp) at (0,0) {comparator};` | `-inn`, `-inp`, `-out` |
| `adc_se` | `\pic (a) at (0,0) {adc_se={ADC}};` | `-in`, `-refp`, `-refn`, `-clk`, `-dout` |
| `adc_diff` | `\pic (fl) at (0,0) {adc_diff={4-bit\\Flash ADC}};` | `-inp`, `-inn`, `-refp`, `-refn`, `-clk`, `-dout` |
| `dac_se` | `\pic (d) at (0,0) {dac_se={DAC}};` | `-din`, `-refp`, `-refn`, `-out` |
| `dac_diff` | `\pic (rd) at (0,0) {dac_diff={R-DAC}};` | `-din`, `-refp`, `-refn`, `-outp`, `-outn` |
| `subadc` | `\pic (s) at (0,0) {subadc={10b SAR$_1$}};` | `-inp`, `-inn`, `-clk`, `-dout` |
| `mux4to1` | `\pic (mx) at (0,0) {mux4to1};` | `-in1`..`-in4`, `-out`, `-sel` |
| `demux1to4` | `\pic (dm) at (0,0) {demux1to4};` | `-in`, `-out1`..`-out4`, `-sel` |

### Filter, DSP, Sources, & Logic
| Stencil | Example | Pins |
| :--- | :--- | :--- |
| `apf_block` | `\pic (apf) at (0,0) {apf_block};` | `-inp`, `-inn`, `-outp`, `-outn` |
| `sum` | `\pic (s) at (0,0) {sum={+/$-$}};` | `-in1`, `-in2`, `-in3`, `-out` |
| `mult` | `\pic (m) at (0,0) {mult};` | `-in1`, `-in2`, `-in3`, `-out` |
| `delay_block` | `\pic (dl) at (0,0) {delay_block={$z^{-L}$}};` | `-in`, `-out` |
| `fir_block` | `\pic (f) at (0,0) {fir_block={7-Tap FIR/$\tau=+0.25\,T_s$}};` | `-in`, `-out`, `-south` |
| `lms_block` | `\pic (l) at (0,0) {lms_block={$\alpha_k$}};` | `-in`, `-out` |
| `dc_source` / `sin_source` / `current_source` | `\pic (v) at (0,0) {dc_source={$V_{\mathrm{DC}}$}};` | `-p`, `-n` |
| `and2` / `nand2` / `or2` / `xor2` | `\pic (g) at (0,0) {and2};` | `-in1`, `-in2`, `-out` |
| `dff` | `\pic (ff) at (0,0) {dff};` | `-d`, `-clk`, `-q`, `-qb`, `-rst` |
| `rise_edge` / `fall_edge` | `\pic (re) at (0,0) {rise_edge};` | `-center` |

---

## 5. Layout Best Practices
1. **Standardized `0.50 cm` (`±0.25 cm`) Differential Pitch:** All differential blocks use `±0.25 cm` pin spacing so differential signal pairs run as straight, parallel lines.
2. **Stagger Differential Vertical Drops:** When two differential wires drop vertically (`|-`), stagger their horizontal tap points by `>= 0.35 cm` so the vertical lines never merge.
3. **Uniform Terminals:** Use `terminal` (`1.6 pt`) consistently at every T-junction and current-summing node.
4. **Circuit Wires vs. Arrows:** Use `adc wire` for analog/circuit connections and reserve `adc arrow` / `adc bus arrow` for digital buses and DSP/block-level flows.
