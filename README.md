# StreamSync-Collaborative-AI-Video-Summarizer

**StreamSync** is a web application that enables users to upload or link long-form videos such as lectures, podcasts, and webinars, and receive AI-generated chapter breakdowns, summaries, and searchable transcripts. Users can collaborate in real time to refine chapter markers and annotate key moments for easy navigation and study.

## Tech Stack
- Express.js (Node.js)
- ASP.NET Core (C#)
- Python

## Requirements
- Transcript generation from video/audio (Hint: Use Whisper or an external transcription API)
- AI chapter and summary generation (Hint: Use LLMs with prompt templates for structured output)
- Collaborative annotation system (Hint: Store user contributions per timestamp)

## Installation

### 1. Express.js Backend
bash
cd express-backend
npm install

Create a `.env` file in `express-backend`:
dotenv
PORT=3000
OPENAI_API_KEY=your_openai_api_key
TRANSCRIPTION_API_KEY=your_transcription_service_key
API_PYTHON_URL=http://localhost:5000


### 2. ASP.NET Core Backend
bash
cd aspnet-core-backend
dotnet restore

Add an `appsettings.Development.json` next to `appsettings.json`:

{
  "ConnectionStrings": {
    "DefaultConnection": "YourDatabaseConnectionString"
  },
  "Jwt": {
    "Key": "YourJwtSecretKey",
    "Issuer": "StreamSync"
  }
}

Run the ASP.NET Core server:
bash
dotnet run --project aspnet-core-backend


### 3. Python Service
bash
cd python-service
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

Create a `.env` in `python-service`:
dotenv
WHISPER_MODEL=base
OPENAI_API_KEY=your_openai_api_key

Start the Python transcription & AI service:
bash
uvicorn main:app --host 0.0.0.0 --port 5000


## Usage
1. Start the Python service (`uvicorn main:app --port 5000`).
2. Start the Express.js backend (`npm start` in `express-backend`).
3. Start the ASP.NET Core backend (`dotnet run` in `aspnet-core-backend`).
4. Open your browser at `http://localhost:3000` to upload videos, view transcripts, chapters, and collaborate on annotations.

## Implementation Steps
1. Scaffold **Express.js** project for file uploads and routing.
2. Integrate Python service using Whisper or an external API for transcription endpoints.
3. Define prompt templates in Python to call an LLM (e.g. OpenAI) for chapter and summary generation.
4. Scaffold **ASP.NET Core** project for user authentication, annotation models, and Web API controllers.
5. Configure Entity Framework Core with a relational database for storing videos, transcripts, chapters, and annotations.
6. Implement collaborative annotation logic: store each user’s timestamped notes and revisions.
7. Wire up the front-end (served by Express.js) to consume Express, ASP.NET Core, and Python service APIs.
8. Write unit and integration tests for each layer (Express routes, ASP.NET Core controllers, Python AI modules).

## API Endpoints

### Express.js Endpoints
- POST   /api/videos/upload            Upload a new video or link
- GET    /api/videos/:id/transcript    Retrieve generated transcript
- GET    /api/videos/:id/chapters      Retrieve AI-generated chapters

### ASP.NET Core Endpoints
- GET    /api/annotations/:videoId     List annotations for a video
- POST   /api/annotations/:videoId     Add or refine an annotation

### Python Service Endpoints
- POST   /api/transcribe               Generate transcript from media
- POST   /api/generate-chapters        Generate chapters and summaries