# CLIPGEN — AI Clip Studio

A first working browser-based prototype for an AI-assisted short-form video clipping studio.

## Included in this build
- Local video upload and browser preview
- Video duration detection
- Candidate highlight ranges (heuristic placeholders, **not actual AI/virality detection**)
- Manual clip creation and trim-point adjustment
- MP4 clip export using FFmpeg.wasm
- Aspect-ratio and editing-style controls (UI settings; rendering presets are not yet wired)
- Responsive desktop/tablet/mobile layout

## Requirements
- Node.js 20.19+ or 22.12+
- npm
- A modern browser with WebAssembly support

## Run locally
```bash
npm install
npm run dev
```

## Production build
```bash
npm run build
npm run preview
```

## Important limitations in this MVP
The “Find best moments” button currently generates reviewable candidate segments based on video duration. It does **not** inspect speech, emotion, scene changes, engagement, or actual platform virality. The numeric match labels are prototype placeholders, not measured predictions. Automatic speech transcription, word-level animated captions, smart reframing, speaker tracking, sound-effect placement, background music, cloud projects/accounts, and mobile-native packaging are planned integrations—not implemented yet. Caption and style panels are previews only.

FFmpeg.wasm runs in the browser and can be memory-intensive. Large videos may fail; test with a short MP4 first. The first export downloads the FFmpeg core from unpkg, so an internet connection is required for initial setup. Only export videos you own or have permission to edit and publish.

## Next milestones
1. Real transcription pipeline (Whisper-compatible service) and timestamped caption generation.
2. Audio/scene analysis to rank candidate moments with transparent reasons.
3. FFmpeg render graph for aspect ratio, captions, zoom, transitions, audio ducking and SFX.
4. Project persistence, authentication and cloud storage.
5. Desktop packaging (Tauri) and mobile app shell (Capacitor or native clients).
6. Job queue and scalable server-side rendering for long videos.

## Suggested architecture for production
- Frontend: React + TypeScript + Vite
- Media engine: FFmpeg (browser for previews/small clips; server workers for reliable long renders)
- AI services: speech-to-text plus a separate moment-ranking pipeline
- Backend: Supabase Auth, Postgres, Storage and Edge Functions for metadata/jobs
- Render workers: containerized FFmpeg workers with a queue; never put long renders in a request/edge function
