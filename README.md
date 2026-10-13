# Aria Lombardia — Air Quality Sonification

A real-time sonification tool that transforms air pollution data from ARPA Lombardia (the regional environmental agency) into distinguishable synthetic vowel sounds, enabling auditory exploration of air quality across monitoring stations in Lombardy, Italy.

Developed as a final project for the **MIDI Programming** course (Laurea Magistrale in Informatica, Università degli Studi di Milano).

## Live Demo

👉 **[[Try it here](https://d4ve99.github.io/Arpa-sonification/)](#)** *(insert your GitHub Pages link)*

## Concept

Each of the five monitored pollutants is mapped to an Italian vowel, following the **A-E-I-U-O** circular path on the F1-F2 acoustic vowel space:

| Vowel | Pollutant | Mnemonic | Frequency |
|-------|-----------|----------|-----------|
| **A** | NO₂ (Nitrogen Dioxide) | biossido di **A**zoto | 220 Hz |
| **E** | PM10 | pi**E**mmedieci | 330 Hz |
| **I** | PM2.5 | part**I**celle sospese | 440 Hz |
| **U** | CO (Carbon Monoxide) | monossido di Carbonio | 550 Hz |
| **O** | O₃ (Ozone) | **O**zono | 660 Hz |

Pollutant concentration modulates the **pitch** (exponential scaling within each vowel's register), so higher pollution → higher pitch. This creates an intuitive mapping: each pollutant has a recognizable vowel timbre, and its severity is conveyed by how "high" it sounds.

## Sound Synthesis

The audio engine uses a **source-filter model** implemented entirely with the Web Audio API:

1. **Source**: A sawtooth oscillator generates a harmonically rich signal
2. **Spectral tilt**: A -12 dB/octave lowpass shapes the glottal source
3. **Formant filters**: 5 parallel bandpass filters (BiquadFilterNode) model the vocal tract resonances, with per-vowel gain profiles for realistic spectral energy distribution
4. **Vibrato**: Subtle frequency modulation (~5 Hz, ±3%) aids vowel recognition
5. **Amplitude envelope**: Attack-sustain-release shaping for natural onset/offset

Formant frequencies are based on standard acoustic phonetics data for Italian vowels.

## Features

- **Interactive map** of all ARPA Lombardia monitoring stations (Leaflet.js)
- **Real-time data** from the ARPA Lombardia SODA API (Socrata Open Data), with fallback to simulated values
- **Two playback modes**:
  - *Sequential* — vowels play one at a time in A-E-I-U-O order
  - *Chord* — all five vowels sound simultaneously
- **Auto-play on station selection** — sound is contextual to the location click
- **Radar glyph popup** — a radar plot with pollutant values appears directly on the map at the clicked station
- **Solo mode** — click any pollutant card to hear it in isolation
- **Mnemonic aids** — highlighted letters in Italian pollutant names reinforce vowel-pollutant associations
- **Progressive loading** — instant display with hardcoded station coordinates, enriched in the background with live API data

## Technical Stack

- **Pure HTML/CSS/JS** — single self-contained file, no build step
- **Web Audio API** — formant synthesis, oscillators, filters, gain nodes
- **Leaflet.js** — interactive map with custom markers and popups
- **SODA API** — real-time environmental data from dati.lombardia.it

## Usage

1. Open the HTML file in any modern browser (Chrome/Firefox/Edge recommended)
2. Click on a station marker on the map
3. The sonification starts automatically and a radar glyph popup appears
4. Use the ▶/■ button to toggle playback
5. Switch between *Sequenza* and *Accordo* modes in the sidebar
6. Click individual pollutant cards at the bottom to hear them solo

No installation or server required — everything runs client-side.

## Data Sources

Air quality data is fetched from the [ARPA Lombardia Open Data portal](https://www.dati.lombardia.it/):
- **Station registry**: `ib47-atvt` (active sensors and station metadata)
- **Sensor readings**: `nicp-bhqi` (latest measured values)

## Project Structure

```
├── index.html          # Complete application (single file)
└── README.md           # This file
```

## License

This project was developed for academic purposes. Data is provided by ARPA Lombardia under their open data license.

## Author

**Davide Cogliati** — Università degli Studi di Milano, Laurea Magistrale in Informatica
