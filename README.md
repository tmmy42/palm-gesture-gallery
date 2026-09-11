# Palm — Gesture Gallery

Open your palm to the camera and a photo from your uploaded collection springs onto your hand and follows it.

A single static `index.html` — no build step, no framework. Hand tracking runs client-side via [MediaPipe Tasks Vision](https://ai.google.dev/edge/mediapipe/solutions/vision/hand_landmarker) (`HandLandmarker`, GPU-accelerated), and the follow / pop-in / pop-out motion is driven by the [`motion`](https://motion.dev) library, both loaded from CDN.

## Run locally

```bash
python3 -m http.server 4173
```

Then open `http://localhost:4173` — the camera requires a secure context (`localhost` or `https`).

## How it works

- **Enable Camera** toggles `getUserMedia` on/off, fully releasing the camera when off.
- **Upload Photos** adds images to a pool; each time an open palm is detected, one is picked at random and springs onto the hand.
- Position and size follow the palm via a persistent spring integrator (framerate-independent); appearance/disappearance use `motion`'s `animate()` on a single scale value.

All motion tuning (spring stiffness/damping/mass, detection cadence) lives in one `const MOTION = {...}` block near the top of the `<script>` in `index.html`.
