# 4CT – Four-Choice Test for song preference

4CT is a Python GUI for running four-choice song preference experiments with birds (e.g. zebra finches and budgerigars). Four perches are connected to an Arduino. When a bird lands on a perch, the program plays the song assigned to that perch through the matching speaker, counts the visit, and logs every event with a millisecond timestamp.

Developed by Marco of the Rhythm & Pitch group, Leiden University.

## Features

- Four songs (A–D), each with one or more audio files (`.wav` / `.mp3`)
- Assignment of songs to perches/speakers, with up to five scheduled position switches during the day to control for side bias
- Live perch counts per song
- Adjustable perch timeout (ms), sent to the Arduino
- Full experimental log, exportable as `.txt` or `.csv`
- Optional automatic save of the log at midnight
- Runs without an Arduino connected (for testing the interface)

## Requirements

- Python 3.11 (tested with 3.11.8)
- Packages listed in `requirements.txt`: `pygame`, `pyserial` (`tkinter` comes with Python)
- An Arduino running the 4CT firmware (see [Hardware](#hardware))
- Windows is recommended; the window icon is Windows-only, but the program also runs on other systems

## Installation

```bash
git clone https://github.com/Maiolini-M/4CT---Behavioural-biology-Leiden.git
cd 4CT---Behavioural-biology-Leiden
pip install -r requirements.txt
```

Or with conda:

```bash
conda create -n 4ct python=3.11
conda activate 4ct
pip install -r requirements.txt
```

## Usage

1. Connect the Arduino by USB. It is detected automatically at start-up; if none is found, the program says so in the console and runs without hardware.
2. Start the program:
   ```bash
   python 4CT.py
   ```
3. Fill in the experiment name, species, perch timeout, and start and end times.
4. Select the audio files for Song A–D.
5. Set the starting speaker position and, optionally, the times and positions of the switches.
6. Press **START**. Press **END** to stop the session and export the log.

Detailed instructions are in the [`Guideline`](Guideline) folder (also available in the program under *Help → Guidelines*).

## Hardware

The Arduino firmware is in the `Arduino` folder. *(Add the `.ino` sketch, a wiring diagram and a parts list here.)*

Serial communication runs at 115200 baud. The program sends these commands:

| Command | Meaning |
|---|---|
| `n` | Start counting |
| `p` | Pause |
| `r` | Reset |
| `c` | Clear counters |
| `sd<ms>` | Set perch timeout in milliseconds |
| `sa<speaker><0/1>` | Switch speaker 1–4 off (0) or on (1) |

The Arduino reports a perch landing as a letter for the perch (`A`–`D`, perch 1–4) followed by the visit count, e.g. `A12`.

## Log format

Each event is logged on one line:

```
<event number>_<experiment name>_<species>_<YYYY-MM-DD_HH:MM:SS.mmm>_<event>
```

At START, the program logs the settings (species, timeout, selected files, times). At END, it logs the speaker positions and the counts per song for each period.

## Citation

If you use 4CT in your research, please cite:

> Maiolini, M. (2026). *4CT: Four-Choice Test* (Version 1.0.0) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.XXXXXXX

## License

Released under the MIT License (see `LICENSE`).
