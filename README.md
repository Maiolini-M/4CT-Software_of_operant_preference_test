# 4CT – Software for four-choice operant song preference tests
[![DOI](https://zenodo.org/badge/1401643650.svg)](https://doi.org/10.5281/zenodo.23103111)

4CT is a Python GUI for running four-choice operant song preference experiments with birds (e.g. zebra finches and budgerigars). Four perches are connected to an Arduino. When a bird lands on a perch, the program plays the song assigned to that perch through the matching speaker, counts the visit, and logs every event with a millisecond timestamp.

Developed by Marco Maiolini (Rhythm & Pitch group, Leiden University).

## Features

- Four songs (A–D), each with one or more audio files (`.wav` / `.mp3`)
- Assignment of songs to perches/speakers, with up to five scheduled position switches during the day to control for side bias
- Live perch counts per song
- Adjustable perch timeout (ms), sent to the Arduino
- Full experimental log, exportable as `.txt` or `.csv`
- Optional automatic save of the log at midnight
- Runs without an Arduino connected (for testing the interface)

## Download (Windows, no Python needed)

A ready-to-run `4CT.exe` is attached to each [release](https://github.com/Maiolini-M/4CT-Software_of_operant_preference_test/releases). Download it and double-click to start.

## Running from source

Requirements:

- Python 3.11 (tested with 3.11.8)
- Packages listed in `requirements.txt`: `pygame`, `pyserial` (`tkinter` comes with Python)
- Windows is recommended; the window icon is Windows-only, but the program also runs on other systems

```bash
git clone https://github.com/Maiolini-M/4CT-Software_of_operant_preference_test.git
cd 4CT-Software_of_operant_preference_test
pip install -r requirements.txt
python 4CT.py
```

Or with conda:

```bash
conda create -n 4ct python=3.11
conda activate 4ct
pip install -r requirements.txt
python 4CT.py
```

## Usage

1. Connect the Arduino by USB. It is detected automatically at start-up; if none is found, the program shows a warning and runs without hardware.
2. Start the program.
3. Fill in the experiment name, species, perch timeout, and start and end times.
4. Select the audio files for Song A–D.
5. Set the starting speaker position and, optionally, the times and positions of the switches.
6. Press **START**. Press **END** to stop the session and export the log.

Detailed instructions are in the [`Guideline`](Guideline) folder (also available in the program under *Help → Guidelines*). Diagrams of the set-up logic are in [`Logic_Schemes`](Logic_Schemes).

## Hardware

4CT communicates with an Arduino that reads the four perch microswitches and switches the four speaker channels. The Arduino firmware was developed separately by Peter Numberi (LION/ELD, Leiden University) and is not included in this repository; contact the author if you need it. Any firmware that follows the serial protocol below will work with 4CT.

Serial communication runs at 115200 baud (8N1). Commands are terminated with CR/LF. The program sends:

| Command | Meaning |
|---|---|
| `n` | Run mode |
| `p` | Stop mode |
| `c` | Clear controller settings (revert to defaults) and reset |
| `r` | Reset controller, keeping settings |
| `sd<ms>` | Set perch timeout in milliseconds (1–999) |
| `sa<channel><0/1>` | Switch audio channel 1–4 |

The Arduino reports a perch landing as a letter for the perch (`A`–`D`, perch 1–4) followed by the visit count, e.g. `A12`.

## Log format

Each event is logged on one line:

```
<event number>_<experiment name>_<species>_<YYYY-MM-DD_HH:MM:SS.mmm>_<event>
```

At START, the program logs the settings (species, timeout, selected files, times). At END, it logs the speaker positions and the counts per song for each period.

## Citation

If you use 4CT in your research, please cite:

> > Maiolini, M. (2026). *4CT: Software for four-choice operant song preference tests* (Version 1.0) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.23103168

You can also use the **Cite this repository** button on the right side of the GitHub page.

## License

Released under the MIT License (see [`LICENSE`](LICENSE)).
