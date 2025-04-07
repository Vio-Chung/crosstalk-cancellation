# Crosstalk Cancellation with External Speakers
This project implements a simple crosstalk cancellation system designed for stereo playback of binaural audio over a pair of Genelec 8010A speakers. By simulating inverted and delayed signal paths between channels, this technique aims to preserve spatial cues that are typically lost when playing binaural recordings through speakers.

## 🔧 My Setup

- **Speaker Placement**: Genelec 8010A pair in equilateral triangle configuration, 71 cm between speakers and to the listener.
- **Crosstalk Distance**: ~77.5 cm between opposing speaker and ear (cross path).
- **Test Tone**: Stereo signal with 440 Hz (left) and 445 Hz (right) sine waves for clarity in spatial separation.

## 🧪 Method

- **Delay Calculation**: Based on physical distances and the speed of sound (343 m/s), compute delays in sample units for both direct and cross paths.
- **Impulse Response Design**:
  - `h_ll` / `h_rr`: Direct path filters with delayed impulses.
  - `h_lr` / `h_rl`: Crosstalk filters with delayed, inverted, and attenuated impulses (-0.5 gain).
- **Filtering**: Applied using `scipy.signal.lfilter()` to generate stereo output:
  ```python
  left_processed = lfilter(h_ll, 1.0, tone_left) + lfilter(h_lr, 1.0, tone_right)
  right_processed = lfilter(h_rr, 1.0, tone_right) + lfilter(h_rl, 1.0, tone_left)


## 🎧 Demo & Evaluation
After generating a test tone, the same filtering approach was applied to a KU100 binaural recording (“Binaural Recording At Cafe.wav”). Listening tests were conducted under a home speaker setup.

## ✅ Results:
Original binaural recording sounded flat and muddy through speakers, with noticeable phase issues.

Processed version using crosstalk cancellation brought back spatial clarity, enhancing immersion and depth.

AB comparison revealed sounds appearing from more precise and natural directions—an experience closer to headphone playback but on speakers.

## 📁 Files
- `XTalk.ipynb` -- main script
- `Pipfile` & `Pipfile.lock` -- Pipenv
- `binaural_test_tone.wav` -- test tone
- `Binaural Recording At Cafe.wav` -- KU100 binaural recording
- `xtalk_Processed_Binaural_Cafe.wav` -- crosstalk processed binaural audio
