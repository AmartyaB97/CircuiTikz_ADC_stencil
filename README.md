# Visio-Style CircuiTikZ Stencil Library (`adc_stencils`)

A reusable, monochrome-by-default LaTeX/TikZ stencil library (`adc_stencils.sty`) for drawing publication-grade analog, mixed-signal, and ADC schematics with named port anchors and orthogonal routing.

## Features
- **TikZ `pic` Stencils:** Drop blocks onto any coordinate and wire directly between named pins (`-inp`, `-inn`, `-outp`, `-outn`, `-clk`, `-dout`, etc.).
- **Standardized `0.50 cm` (`±0.25 cm`) Differential Pitch:** Differential blocks (`apf_block`, `diff_opamp`, `adc_diff`, `dac_diff`, `subadc`, `sw_open` pairs) align cleanly for straight, parallel differential routing.
- **Included Stencil Categories:**
  - **Interconnect & Rails:** `terminal`, `ioport`, `iopin`, `noconn`, `jumper`, `bus_tick`, `vdd`, `gnd`
  - **Passives & Switches:** `res`, `vres`, `cap`, `vcap`, `ind`, `sw_open`, `sw_close`, `sw_2port`
  - **Atomic Transistors:** `nmos`, `pmos`
  - **Amplifiers & Converters:** `opamp`, `diff_opamp`, `comparator`, `adc_se`, `adc_diff`, `dac_se`, `dac_diff`, `subadc`, `mux4to1`, `demux1to4`
  - **Filter & DSP Blocks:** `apf_block`, `sum`, `mult`, `delay_block`, `fir_block`, `lms_block`
  - **Sources, Logic, & Edges:** `dc_source`, `sin_source`, `current_source`, `and2`, `nand2`, `or2`, `xor2`, `dff`, `rise_edge`, `fall_edge`

## Visual Catalog
See [stencil_gallery.pdf](stencil_gallery.pdf) (or [stencil_gallery-1.png](stencil_gallery-1.png) and [stencil_gallery-2.png](stencil_gallery-2.png)) for every stencil with labeled pin anchors, an end-to-end differential CT-DT ADC frontend example, and a transistor-level differential pair example.

## Installation
Clone the repository and symlink `adc_stencils.sty` into your local `~/texmf` tree:

```bash
git clone https://github.com/<your-username>/adc-circuitikz-stencils.git
mkdir -p ~/texmf/tex/latex/adc_stencils
ln -sf "$(pwd)/adc-circuitikz-stencils/adc_stencils.sty" ~/texmf/tex/latex/adc_stencils/adc_stencils.sty
```

## Usage
```latex
\documentclass{article}
\usepackage[american]{circuitikz}
\usepackage{adc_stencils}

\begin{document}
\begin{tikzpicture}[>=Latex]
    \pic (pin_p) at (-2.0, 0.25) {ioport={$V_{\mathrm{in}+}$}};
    \pic (pin_n) at (-2.0, -0.25) {ioport={$V_{\mathrm{in}-}$}};
    \pic (apf)   at (0, 0) {apf_block};

    \draw[adc wire] (pin_p-out) -- (apf-inp);
    \draw[adc wire] (pin_n-out) -- (apf-inn);
\end{tikzpicture}
\end{document}
```
