> **This project has been archived.** WaxOn and WaxOff have been merged into a single app — [WaxOn/WaxOff](https://github.com/sevmorris/WaxOnWaxOff).

**Podcast Audio Prep for macOS**

WaxOn prepares raw podcast recordings for editing in a DAW. It handles high-pass filtering, loudness normalization, phase rotation, and brick-wall limiting in a single drag-and-drop workflow, outputting 24-bit WAV files.

WaxOn does not perform noise reduction, de-essing, or any other restoration — it's a first-step ingest processor that gets your raw recordings into a consistent, workable format before further prep or editing.

## Design Philosophy

WaxOn is focused on podcast audio prep. It exposes the controls that matter for that job and keeps sensible defaults for everything else. The goal is a low-friction workflow: drop your files in, configure what you care about, and get to editing.

## Download

**[WaxOn v2.6 (DMG)](https://github.com/sevmorris/WaxOn/releases/latest/download/WaxOn-v2.6.dmg)** · **[Manual](https://sevmorris.github.io/WaxOn/)**

> ⚠️ **Important — Read Before First Launch**
>
> macOS will block the app with a malware warning because it is not notarized with Apple. After mounting the DMG and dragging WaxOn to Applications, **you must run this command in Terminal:**
>
> ```
> xattr -cr /Applications/WaxOn.app
> ```
>
> Without this step, macOS will refuse to open the app.

## Features

- **High-Pass Filter**: Configurable cutoff (40–90 Hz, default 80 Hz) removes low-frequency rumble and handling noise
- **Loudness Normalization**: Optional two-pass EBU R128 normalization with configurable target (-35 to -16 LUFS, default -30). Uses linear gain for transparent level matching across files. True peak target respects your ceiling setting.
- **Brick-Wall Limiting**: Configurable ceiling (-3 to -1 dB) with 2x oversampled true peak limiting
- **Phase Rotation**: Automatic 200 Hz allpass to reduce peak asymmetry and improve headroom
- **Mono or Stereo Output**: Mono with left/right channel selection, or stereo passthrough
- **Sample Rate Conversion**: 44.1 kHz or 48 kHz output
- **Drag & Drop**: Drop audio files onto the window to process
- **Batch Processing**: Process multiple files in parallel (up to 3 concurrent jobs) with per-file progress
- **Mix**: Select 2 or more files and mix them down to a single processed output — mixed with normalized amix, then run through the full WaxOn pipeline
- **Input Validation**: Unsupported file formats are rejected automatically
- **File Reordering**: Drag to reorder files in the processing queue
- **Custom Output Directory**: Optionally set a dedicated output folder
- **Reveal in Finder**: Click to reveal processed output files
- **Waveform Preview**: Select a file to view its waveform with dB scale
- **File Stats**: After selecting a file, a stats strip below the waveform shows format, sample rate, channels, bit depth, duration, bit rate, RMS, peak, crest factor, and integrated LUFS (ITU-R BS.1770)
- **Mix Progress**: The waveform panel shows a live phase indicator (Mixing → Filtering → Analyzing loudness → Normalizing → Limiting) while a mix is rendering

## System Requirements

- macOS 14.0 (Sonoma) or later
- FFmpeg (bundled or installed via Homebrew)

## Usage

1. Launch WaxOn
2. Configure your settings:
   - **Sample Rate**: 44.1 kHz or 48 kHz
   - **Output**: Mono or Stereo
   - **Channel** (mono only): Left or Right from stereo source
   - **Limiter**: Ceiling level (e.g., -1.0 dB)
3. Drag and drop audio files onto the window
4. Click "Process" — or select 2+ files and click "Mix" to combine them into a single processed output
5. Output files are saved alongside the originals with a `-waxon` suffix (or to your configured output directory)

## Output Naming

```
{original-name}-{samplerate}waxon-{limit}dB.wav
```

Example: `episode-01-44kwaxon-1dB.wav`

Mix output:

```
mix-{N}-files-{samplerate}waxon-{limit}dB.wav
```

Example: `mix-2-files-44kwaxon-1dB.wav`

## Additional Settings

- **High Pass**: High-pass filter cutoff frequency (default 80 Hz, range 40–90 Hz)
- **Loudness Norm**: Enable EBU R128 loudness normalization
- **Target**: Integrated loudness target (default -30 LUFS, range -35 to -16 LUFS)
- **Output Directory**: Set a custom output folder (default: saves alongside source files)

## Processing Pipeline

WaxOn uses FFmpeg with a multi-pass pipeline:

1. **High-pass filtering**, channel selection (if mono), phase rotation, and resampling to target sample rate
2. **Loudness normalization** (if enabled) — two-pass EBU R128 analysis followed by linear gain application
3. **Brick-wall limiting** with 2x oversampled true peak control

Output format: 24-bit WAV

## Companion App

[WaxOff](https://github.com/sevmorris/WaxOff) finalizes your podcast mix for distribution — EBU R128 loudness normalization, optional phase rotation, and MP3 encoding.

**Workflow**: Raw recordings → **WaxOn** → Edit in DAW → **WaxOff** → Distribute

## License

Copyright © 2026. This app was designed and directed by Seven Morris, with code primarily generated through AI collaboration using [OpenClaw](https://openclaw.ai) and Claude (Anthropic).

This program is free software: you can redistribute it and/or modify it under the terms of the [GNU General Public License v3.0](LICENSE).

## A Note on AI

I'm a freelance audio engineer, not a software developer. These tools exist because AI made it possible for me to build things I couldn't build alone. It's exciting, but complicated.

The current app icon is my own (very minimal) design. AI can build the software, but I can still make the art myself, and I think that's worth doing.

AI-assisted development raises real questions about labor displacement, resource consumption, and the concentration of power in a handful of tech companies. I don't have clean answers. I do think it matters that the people using these tools are honest about the trade-offs rather than pretending they don't exist.
