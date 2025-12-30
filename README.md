# Autodubbing

**Automatic Video Dubbing Pipeline**

## Overview

Autodubbing is an automated video dubbing system that takes a video in one language and produces a dubbed version in another language while preserving the original speakers' voice characteristics. The pipeline combines speech recognition, translation, text-to-speech synthesis, and voice conversion technologies.

🔗 **Repository:** https://github.com/muneer-kayali/autodubbing

## Features

- 🎬 Audio extraction from video files (FFmpeg)
- 🎤 Speech-to-text transcription with speaker diarization (Whisper)
- 🧠 Intelligent speaker identification and segment grouping (GPT-4)
- 🌍 Text translation to target language (DeepL API)
- 🔊 Text-to-speech synthesis (Google Cloud TTS)
- 🎭 Voice conversion to match original speakers (SEED-VC)
- ⏱️ Audio segment alignment with original timing
- 🎥 Final video assembly with dubbed audio

## Pipeline Stages

### 1. Audio Extraction
- Extracts audio track from input video using FFmpeg
- Converts to MP3 format for processing

### 2. Transcription
- Uses Stable Whisper (large model) for speech-to-text
- Voice Activity Detection (VAD) for accurate segment boundaries
- Outputs timestamped text segments

### 3. Speaker Diarization
- GPT-4 analyzes transcribed segments to identify different speakers
- Groups consecutive segments by the same speaker
- Maintains timing information for each segment

### 4. Translation
- DeepL API translates text to target language (default: Danish)
- Preserves segment structure and timing metadata

### 5. Text-to-Speech
- Google Cloud Text-to-Speech synthesizes translated text
- Uses neural voices (da-DK-Neural2-D for Danish)
- Generates audio for each translated segment

### 6. Voice Conversion
- SEED-VC converts synthesized speech to match original speaker voices
- Extracts reference audio for each speaker from original
- Adjusts speech rate to match original segment duration (0.75x - 1.25x)

### 7. Audio Assembly
- Creates silent audio track matching original duration
- Overlays converted segments at their original timestamps
- Exports final dubbed audio

### 8. Video Assembly
- Replaces original audio track with dubbed audio using FFmpeg
- Preserves original video stream without re-encoding

## Requirements

**Python 3.10+**

### Core Dependencies

| Package | Purpose |
|---------|---------|
| `stable-whisper` | Speech recognition |
| `openai` | GPT-4 for speaker diarization |
| `deepl` | Translation API |
| `google-cloud-texttospeech` | TTS synthesis |
| `pydub` | Audio manipulation |
| `pysrt` | SRT subtitle parsing |
| `ffmpeg` | Audio/video processing (system install) |

### Voice Conversion

- [SEED-VC](https://github.com/Plachtaa/seed-vc)
- PyTorch (Deep learning backend)

## API Keys Required

| Service | Purpose |
|---------|---------|
| OpenAI API Key | GPT-4 speaker diarization |
| DeepL API Key | Text translation |
| Google Cloud JSON | Text-to-Speech service |

## Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/muneer-kayali/autodubbing
   ```

2. **Create virtual environment:**
   ```bash
   python -m venv .venv
   .venv\Scripts\activate        # Windows
   source .venv/bin/activate     # Linux/Mac
   ```

3. **Install dependencies:**
   ```bash
   pip install stable-whisper openai deepl google-cloud-texttospeech pydub pysrt
   ```

4. **Install FFmpeg:**
   - Windows: Download from https://ffmpeg.org/download.html
   - Add to system PATH

5. **Clone SEED-VC for voice conversion:**
   ```bash
   git clone https://github.com/Plachtaa/seed-vc seed-vc-main
   ```

6. **Configure API keys in the notebook:**
   - Set `OPENAI_API_KEY` environment variable
   - Set `GOOGLE_APPLICATION_CREDENTIALS` to your Google Cloud JSON file
   - Enter DeepL `auth_key` in the translation cell

## Usage

1. Place input video as `video_input.mp4` in the project directory

2. Open `autodubbing.ipynb` in Jupyter Notebook/Lab

3. Run cells sequentially:
   - Audio extraction
   - Transcription (may take several minutes for large files)
   - Speaker diarization
   - Translation
   - Text-to-speech synthesis
   - Voice conversion
   - Final assembly

4. Output: `video_output.mp4` with dubbed audio

## File Structure

```
autodubbing/
├── autodubbing.ipynb          # Main pipeline notebook
├── video_input.mp4            # Input video (user provided)
├── audio_input.mp3            # Extracted audio
├── tempfiles/                 # Temporary audio segments
│   ├── segment0.wav           # Original audio segments
│   ├── translated_segment0.wav # TTS output
│   └── ...
├── speaker_audio/             # Reference audio per speaker
│   ├── speaker_1.wav
│   └── speaker_2.wav
├── vc_segments/               # Voice-converted segments
│   ├── segment_0/
│   └── ...
├── output_with_timing.mp3     # Final dubbed audio
└── video_output.mp4           # Final output video
```

## Limitations

- Voice conversion speed adjustment limited to 0.75x - 1.25x
- Requires significant computational resources for Whisper large model
- Speaker diarization relies on GPT-4 inference (API costs apply)
- Target language hardcoded to Danish (modifiable in code)

## Credits

- [Stable Whisper](https://github.com/jianfch/stable-ts)
- [SEED-VC](https://github.com/Plachtaa/seed-vc)
- [OpenAI GPT-4](https://openai.com/)
- [Google Cloud Text-to-Speech](https://cloud.google.com/text-to-speech)
- [DeepL Translation API](https://www.deepl.com/pro-api)

## License

See repository for license information.
