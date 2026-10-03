# Next Slide

Say "next slide" and ProPresenter advances. Hands-free slide control for when you're presenting.

It listens to your mic, transcribes what you say with a local Whisper server, and when it hears "next slide" it tells ProPresenter to go to the next slide in the focused presentation. That's the same thing the right arrow key does. Everything runs on your own Mac. Nothing goes to the cloud.

## What you need

- **macOS** with **ProPresenter** (tested on ProPresenter 21.4), with its Network API turned on: ProPresenter > Settings > Network > Enable Network.
- **[uv](https://docs.astral.sh/uv/)**, which installs the Python bits automatically on first run.
- A **[whisper.cpp](https://github.com/ggml-org/whisper.cpp) server** on `localhost:2022` that serves the OpenAI-style `/v1/audio/transcriptions` route. For example, `whisper-server --port 2022 --inference-path /v1/audio/transcriptions -m ggml-base.en.bin`.

## Run it

```bash
uv run next_slide.py
```

Your terminal app will ask for microphone access the first time. It prints everything it hears. Say "next slide" and the slide moves. Press Ctrl+C to quit.

You don't have to tell it ProPresenter's port. ProPresenter can change ports between launches, so the app finds it by itself every time it starts.

## Test without talking

```bash
say -o test.wav --data-format=LEI16@16000 "Let's get started. Next slide."
uv run next_slide.py --file test.wav
```

This really does press next slide in ProPresenter.

## How it behaves

- A short pause (0.4 s) ends a phrase. The slide moves about 1 to 1.5 seconds after you stop talking.
- "Next slide" works anywhere in a sentence. Phrases like "the next steps" don't trigger it.
- "Next slide, next slide" moves two slides.
- It hears speakers too. If a video or another app in the room says "next slide," it will advance.

## License

MIT
