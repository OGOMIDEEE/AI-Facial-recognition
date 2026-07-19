# AI Facial Recognition

A browser-based facial detection and analysis app built with Angular and [face-api.js](https://github.com/justadudewhohacks/face-api.js). It uses your webcam to capture a frame and, entirely client-side (no server, no image upload), detects faces and estimates age, gender, and emotional expression for each one.

## Features

- **Live webcam feed** — accesses the browser's camera via `getUserMedia`
- **One-click capture** — grabs a frame from the video stream onto a canvas
- **Face detection & analysis** — for each detected face, estimates:
  - Age
  - Gender
  - Emotional expression breakdown (happy, sad, angry, surprised, etc.)
- **Fully client-side** — face detection models run in the browser via TensorFlow.js (through face-api.js); no images ever leave the device
- **NgRx store scaffolding** — actions/reducers for capturing images and storing detected faces, ready to wire into the UI for state management

## Tech stack

- [Angular 13](https://angular.io/) (CLI-generated project)
- [face-api.js](https://github.com/justadudewhohacks/face-api.js) — face detection, landmarks, age/gender, and expression recognition, built on TensorFlow.js
- [@ngrx/store](https://ngrx.io/) + `@ngrx/effects` for state management
- Bootstrap (via CDN/classes) for layout

### Models used

Pretrained face-api.js models are bundled under `src/assets/models/`:

- `tiny_face_detector` — fast face detection
- `face_landmark_68` (+ tiny variant) — facial landmark detection
- `face_recognition_model` — face descriptor/recognition
- `age_gender_model` — age and gender estimation
- `face_expression_model` — emotion/expression classification
- `mtcnn_model` — alternative face detector
- `ssd_mobilenetv1_model` — alternative face detector

## Project structure

```
src/app/
├── webcam/                  # Core component: camera access, capture, face detection UI
│   ├── webcam.component.ts   # Loads face-api.js models, runs detection
│   └── webcam.component.html # Video/canvas UI + detected face results
├── store/
│   ├── actions.ts             # NgRx actions: captureImage, detectFaces
│   └── reducers.ts            # NgRx reducer for capturedImage/detectedFaces state
src/assets/models/           # Pretrained face-api.js model weights
```

## Getting started

### Prerequisites

- [Node.js](https://nodejs.org/) (compatible with Angular 13, e.g. Node 14–16)
- [Angular CLI](https://angular.io/cli) v13.3.11: `npm install -g @angular/cli@13.3.11`
- A browser with webcam access (and permission granted when prompted)

### Installation

```bash
git clone https://github.com/OGOMIDEEE/AI-Facial-recognition.git
cd AI-Facial-recognition
npm install
```

### Development server

```bash
ng serve
```

Navigate to `http://localhost:4200/`, allow camera access when prompted, and click **Capture Image** to run detection on the current frame.

> Camera access requires a secure context — this works on `localhost` during development, but a production deployment needs to be served over HTTPS.

### Build

```bash
ng build
```

Build artifacts are output to the `dist/` directory. Make sure `src/assets/models/` is included in the deployed build so the models can be loaded at runtime.

### Running unit tests

```bash
ng test
```

Runs unit tests via [Karma](https://karma-runner.github.io).

## How it works

1. On load, `WebcamComponent` loads the required face-api.js models from `assets/models` and starts the webcam stream.
2. Clicking **Capture Image** draws the current video frame onto a hidden canvas.
3. `face-api.js` runs detection on that canvas frame — face bounding boxes, landmarks, age/gender, and expressions.
4. Results are rendered as a list of detected faces with their estimated age, gender, and emotion breakdown.



- Wire the NgRx actions/reducers into the component (currently detection results are held in local component state, not dispatched to the store)
- Add real face recognition/identification (matching against known faces) rather than the placeholder `"Trial Name"` label
- Add continuous/live detection instead of single-frame capture
