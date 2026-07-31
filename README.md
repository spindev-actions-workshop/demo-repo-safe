# Task Tracker (demo monorepo)

A small, fictive Task Tracker web app used as a hands-on repo for the GitHub Actions workshop.
It's a Node.js/Express backend + React frontend npm-workspaces monorepo, intentionally simple, so the
focus stays on the GitHub/Actions features it demonstrates rather than the app itself.

## Structure

```
backend/    Express API (in-memory data), tests via node:test + supertest, Dockerfile
frontend/   React app (Vite), tests via vitest + Testing Library, Dockerfile (nginx)
.github/    CI, CodeQL and dependency review workflows, CODEOWNERS
```

## What this repo showcases

- **npm workspaces monorepo** with independent lint/test/build per package
- **`act`**: run the `ci` workflow locally with Docker (see below)
- **CI workflow** ([.github/workflows/ci.yml](.github/workflows/ci.yml)): lint, test and build both packages, then build both Dockerfiles
- **Dependency review** ([.github/workflows/dependency-review.yml](.github/workflows/dependency-review.yml)): flags newly introduced vulnerable/high-severity dependencies on pull requests
- **Code scanning** ([.github/workflows/codeql.yml](.github/workflows/codeql.yml)): CodeQL analysis for JavaScript/TypeScript
- **CODEOWNERS** ([.github/CODEOWNERS](.github/CODEOWNERS)): required review routing per path
- Ready to be wired up to branch protection rulesets, auto-merge, merge queue, etc. (see the Day 3 workshop material)

## Local development

```bash
# install all workspace dependencies
npm install

# run everything
npm run lint
npm run test
npm run build

# run a single workspace
npm run test --workspace backend
npm run dev --workspace frontend
```

The backend listens on `http://localhost:3001`, the frontend dev server proxies `/api` to it.

## Run with Docker

```bash
docker compose up --build
```

- Frontend: http://localhost:8080
- Backend: http://localhost:3001

## Run the CI workflow locally with act

```bash
act push -W .github/workflows/ci.yml
```

> The `docker` job builds container images inside the workflow - make sure Docker is running before invoking act.
