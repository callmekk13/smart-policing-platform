# Smart Policing Command Platform 

Smart Police Station is a full-stack prototype for modernizing local policing workflows. It includes:

- A modular backend server (Node.js, Express, MongoDB, Socket.IO) providing REST APIs, realtime events, file uploads, and integrations.
- A web client (Vite + React) located in the `client/` folder.
- A mobile client (React Native / Expo) located in the `client-mobile/` folder.

This repository contains all three components and supporting scripts to run the system locally for development and demonstration purposes.

Contents
- Project overview
- Getting started (quick)
- Running each component (server, web client, mobile)
- Environment variables
- API docs & testing
- Seeds and demo data
- Docker & local development with compose
- Deployment guidance
- Contributing and contact

Project overview
----------------

Smart Police Station demonstrates a minimal viable platform for handling citizen complaints, FIRs, SOS/emergency handling, patrol planning, notifications, and officer coordination. It is built to be modular, swap-able (storage, map provider, SMS provider), and easy to run locally for hackathons or prototyping.

Key capabilities
- Role-based authentication for admins, officers, and citizens
- Complaint intake and workflow (create, update, escalate to FIR)
- SOS/emergency flow with realtime notifications and location updates
- Patrol planning and officer tracking via Socket.IO
- File uploads for evidence/documents (local disk or S3)
- Notification & announcement management

Quick start (recommended)
-------------------------
Prerequisites
- Node.js v18+ (LTS recommended)
- npm or yarn
- MongoDB (local or Atlas)

Start the backend server

```bash
cd server
npm install
cp .env.example .env
# edit .env to set MONGO_URI and JWT_SECRET
npm run seed:admin   # optional: seed admin user
npm run dev
```

Start the web client

```bash
cd client
npm install
npm run dev
```

Start the mobile client (if using Expo)

```bash
cd client-mobile
npm install
npm run start
```

Running individual components
-----------------------------

Server
- Code: `server/src`
- Main README: `server/README.md` (contains detailed setup, env vars, API pointers, socket and uploads details)

Web client
- Code: `client/`
- Typical commands: `npm install`, `npm run dev` (uses Vite)

Mobile client
- Code: `client-mobile/`
- Typical commands: `npm install`, `npm run start` (Expo or similar)

Environment variables
---------------------

The server uses a `.env` file in the `server` folder. Common variables:

- `PORT` — API port (default 8000)
- `MONGO_URI` — MongoDB connection string
- `JWT_SECRET` — JWT signing secret
- `CLIENT_URL` — front-end origin
- `AWS_S3_BUCKET`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` — if using S3
- `REDIS_URL` — for Socket.IO scaling (optional)

API docs & testing
-------------------

Detailed API documentation lives in `server/docs/API.md`. For integration testing examples, see `server/docs/API_TESTING.md`.

Seeds and demo data
-------------------

Seed scripts are located in `server/src/seed/`. Use `npm run seed` or `npm run seed:admin` from the `server` folder to populate demo data and an admin user.

Docker & local development with compose
--------------------------------------

You can run the backend alongside a MongoDB container. Example (from `server/` or repo root):

```yaml
version: '3.8'
services:
	mongo:
		image: mongo:6
		volumes:
			- mongo-data:/data/db
		ports:
			- 27017:27017

	api:
		build: ./server
		ports:
			- 8000:8000
		environment:
			- MONGO_URI=mongodb://mongo:27017/smart_police
		depends_on:
			- mongo

volumes:
	mongo-data:
```

Deployment guidance
-------------------

- Use a process manager (PM2) or containers for production.
- Terminate TLS at a reverse proxy (NGINX) and run Node behind it.
- Configure Redis for Socket.IO adapter when scaling across multiple instances.
- Store uploads in S3 or another durable store in production.

Contributing
------------

PRs welcome. Please include tests for new features where applicable, and keep changes focused to a single concern. Run linters and ensure the server and clients still start locally before opening a PR.

Contact / maintainers
---------------------
See repository metadata or open an issue for questions. If you'd like, I can also:

- Add a top-level `docker-compose.yml` tuned to run server + client locally.
- Create a CONTRIBUTING.md with contribution and testing instructions.

License
-------
Add or review the `LICENSE` file in the repository root. If none exists, please add a license before publishing.

References
- Server README: `server/README.md`
- API docs: `server/docs/API.md`
- Web client: `client/`
- Mobile client: `client-mobile/`

# smart-policing-platform
