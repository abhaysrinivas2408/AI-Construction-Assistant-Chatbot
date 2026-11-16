# Bricky – AI Construction Assistant Chatbot

**Bricky** is a full-stack AI chatbot that answers questions on construction: building materials, safety regulations, building codes, project planning and best practices.

It uses a **hybrid answer pipeline**. Every question is first checked against a curated **MongoDB Atlas knowledge base**. Only when there's no stored answer does Bricky ask an LLM (**Claude 3 Haiku via OpenRouter**). Common questions get instant answers, and paid API calls stay low.

![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)
![Vite](https://img.shields.io/badge/Vite-6-646CFF?logo=vite&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-Express%205-339933?logo=nodedotjs&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-47A248?logo=mongodb&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-EC2-FF9900?logo=amazonaws&logoColor=white)

---

## Features

- **Knowledge base first.** Questions are matched against stored Q&A entries by regex on the question and by keywords.
- **LLM fallback.** If nothing matches, the question goes to Claude 3 Haiku with a construction-specific system prompt.
- **Answer source tracking.** Each response says whether it came from the knowledge base (`kb`) or the LLM (`claude`).
- **Resilient API layer.** A 45-second timeout, plus clear user-facing messages for invalid API keys (401), rate limits (429), bad requests (400), model unavailability (404) and network failures.
- **Polished chat UI.** Built with React 19 and Vite, with Framer Motion and Lottie animations, auto-scroll, Enter-to-send, a loading state and a responsive mobile layout with a collapsible menu.
- **Deployed on AWS EC2.**

---

## How it works

```
User question (React chat UI)
        │  POST /api/ask-question
        ▼
Express API
        ├─► MongoDB Atlas knowledge base ── match? ──► answer  (source: "kb")
        │
        └─► no match ──► OpenRouter → Claude 3 Haiku ──► answer  (source: "claude")
```

---

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | React 19, Vite, Axios, Framer Motion, Lottie, React Icons |
| Backend | Node.js, Express 5, Mongoose, Axios, dotenv, CORS |
| Database | MongoDB Atlas |
| AI | Claude 3 Haiku via the OpenRouter API |
| Hosting | AWS EC2 |

---

## Project structure

```
AI-Construction-Assistant-Chatbot/
├── Front-end/
│   ├── src/
│   │   ├── components/chatbot.jsx   # Chat UI and API calls
│   │   ├── styles/                  # Chat and popup styles
│   │   ├── assets/bot.json          # Lottie bot animation
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── index.html
│   └── package.json
└── backend/
    ├── server.js                    # Express app and MongoDB connection
    ├── routes/chatbot.js            # /api/ask-question: KB lookup and LLM fallback
    ├── models/KnowledgeBase.js      # Mongoose schema (question, keywords, answer)
    └── package.json
```

---

## Getting started

### Prerequisites

- Node.js 18+
- A MongoDB Atlas cluster
- An [OpenRouter](https://openrouter.ai) API key

### 1. Backend

```bash
cd backend
npm install
```

Create a `.env` file in `backend/`:

```env
MONGO_URI=your_mongodb_atlas_connection_string
OPENROUTER_API_KEY=your_openrouter_api_key
PORT=5000
```

Start the server:

```bash
node server.js
```

### 2. Frontend

```bash
cd Front-end
npm install
npm run dev
```

> The API address is set in `src/components/chatbot.jsx`. For local development, change it to `http://localhost:5000/api/ask-question`.

### 3. Add knowledge-base entries

Add documents to the `Knowledgebase` collection in MongoDB Atlas:

```json
{
  "question": "What is the curing time for concrete?",
  "keywords": ["curing", "concrete", "time"],
  "answer": "Concrete reaches about 70% of its strength in 7 days and its full design strength in 28 days…"
}
```

---

## API

### `POST /api/ask-question`

```json
// Request
{ "query": "What PPE is required on a construction site?" }

// Response
{ "answer": "Standard PPE includes a hard hat, safety boots, high-visibility vest…", "source": "claude" }
```

---

## Next steps

- Move the API address into a Vite environment variable (`VITE_API_URL`) and serve the API over HTTPS.
- Restrict CORS to the frontend's domain.
- Replace keyword matching with vector search (embeddings) for semantic knowledge-base retrieval (RAG).
- Keep conversation history so follow-up questions have context.

---

## Author

**Abhay Srinivas**, B.E. Computer Science and Engineering, BNM Institute of Technology
[Portfolio](https://p-coral-tau.vercel.app/) · [LinkedIn](https://www.linkedin.com/in/abhay-srinivas-7b46103b3/)
