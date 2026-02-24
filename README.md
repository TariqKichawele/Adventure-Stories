# Interactive Story Generator

A full-stack "choose your own adventure" web application that uses AI to generate branching narratives with multiple paths and endings. Players pick a theme, and the app creates an interactive story powered by OpenAI, where every choice leads to a different outcome.

## Tech Stack

**Frontend**
- React 19 with Vite 7
- React Router for navigation
- Axios for API communication

**Backend**
- Python 3.14 with FastAPI
- LangChain + OpenAI (GPT-4o-mini) for story generation
- SQLAlchemy ORM with SQLite
- Background task processing for async story creation

## Project Structure

```
adventure_stories/
├── backend/
│   ├── core/              # Config, prompts, and story generation logic
│   ├── db/                # Database engine and session management
│   ├── models/            # SQLAlchemy models (Story, StoryNode, StoryJob)
│   ├── routers/           # API route handlers
│   ├── schemas/           # Pydantic request/response schemas
│   ├── main.py            # FastAPI app entrypoint
│   └── .choreo/           # Choreo deployment config
├── frontend/
│   ├── src/
│   │   ├── components/    # React components (StoryGame, StoryGenerator, etc.)
│   │   ├── App.jsx        # Root component with routing
│   │   └── util.js        # Shared utilities
│   └── vite.config.js     # Vite config with dev proxy
└── README.md
```

## How It Works

1. The player enters a story theme (e.g. "fantasy", "sci-fi", "horror")
2. The backend creates a background job and uses LangChain to prompt GPT-4o-mini for a complete branching story structure
3. The generated story is stored as a tree of nodes — each node has narrative content, 2-3 choices, and some nodes are endings (winning or losing)
4. The frontend presents the story one node at a time, letting the player make choices that navigate the tree
5. The player can restart the story or generate a new one at any time

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/stories/create` | Start story generation (returns a job ID) |
| GET | `/api/jobs/{job_id}` | Poll generation job status |
| GET | `/api/stories/{story_id}/complete` | Fetch the full story tree |

## Getting Started

### Prerequisites

- Python 3.14+
- Node.js 18+
- An [OpenAI API key](https://platform.openai.com/api-keys)

### Backend

```bash
cd backend

# Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Create a .env file
cp .env.example .env  # or create manually
```

Add the following to `backend/.env`:

```
DATABASE_URL=sqlite:///database.db
OPENAI_API_KEY=your-openai-api-key
ALLOWED_ORIGINS=http://localhost:5173
```

Start the server:

```bash
python main.py
```

The API will be available at `http://localhost:8000` with interactive docs at `/docs`.

### Frontend

```bash
cd frontend

# Install dependencies
npm install

# Create a .env file
echo "VITE_DEBUG=true" > .env

# Start the dev server
npm run dev
```

The app will be available at `http://localhost:5173`. When `VITE_DEBUG=true`, API requests are proxied to the backend automatically.

## Deployment

The backend includes a [Choreo](https://choreo.dev/) component configuration (`.choreo/component.yaml`) for cloud deployment, with an OpenAI connection reference pre-configured.
