# Sonify

Voice-first Shopify analytics assistant that answers a merchant's questions and produces short audio representations of metrics ("sonifications"), so trends, comparisons, and anomalies can be perceived by ear without reading charts. Built as an accessibility experiment for non-visual commerce analytics.

## What it does

- **Talk + Listen loop.** Press Talk, ask a question ("How are we doing today?"). The agent responds with a short spoken answer plus an optional sonification clip that encodes the same data as sound.
- **Sonification presets.** Pitch encodes value, loudness encodes change magnitude, and distinct earcons mark spikes, dips, and anomalies. `trend_v1` for a single metric, `compare_v1` for two ranges, `multitrack_v1` to layer revenue + orders.
- **Two modes.** Explain Mode (default) narrates the data; Listen Mode favors sonification with short cues ("spike on Feb 24").
- **Accessibility features.** Daily audio briefing, anomaly listen, and A/B comparison of two date ranges by sound.

## Repo layout

- `testapp/` — the Shopify React Router app. Contains the Backboard voice agent (`app/lib/agent/`), the metrics API (`app/lib/metrics/`, `app/routes/api.metrics.*`), the sonification engine and WAV renderer (`app/lib/sonification/`, `app/routes/api.sonify.*`), the ElevenLabs TTS proxy (`app/lib/tts/`), the voice UI components (`app/components/voice/`), Prisma schema, and the `node --test` suites.
- `sonify_dashboard.py` — a standalone matplotlib + NumPy/SciPy demo that sonifies 30 days of traffic vs revenue (continuous theremin-style tone or per-day plucks) as a quick illustration of the sonification idea. Requires `numpy`, `matplotlib`, `scipy`, and `sounddevice`.
- `PROJECTSPEC.md` — the full product spec (UX, architecture, data model, sonification design, agent schema, demo script).

## Tech stack

React Router 7 (Remix successor), React 18, Shopify App Bridge + `@shopify/shopify-app-react-router`, Prisma, Vite, TypeScript. Backboard for the voice agent, ElevenLabs for TTS. Python (matplotlib/NumPy/SciPy/sounddevice) for the standalone dashboard demo.

## Run the Shopify app

The app lives in `testapp/`. See `testapp/README.md` for the template quick start; in summary:

```shell
cd testapp
npm install
shopify app dev
```

You need the [Shopify CLI](https://shopify.dev/docs/apps/tools/cli/getting-started) and a Shopify Partner account / dev store. Copy `testapp/.env.example` to `testapp/.env` and fill in `BACKBOARD_API_KEY` and `ELEVENLABS_API_KEY` to enable the agent and TTS.

## Run the Python dashboard

```shell
pip install numpy matplotlib scipy sounddevice
python sonify_dashboard.py
```

## Tests

```shell
cd testapp
npm test                 # voice + metrics + sonification suites
npm run test:agent       # agent / tool registry
```

## License

MIT. See [LICENSE](LICENSE).
