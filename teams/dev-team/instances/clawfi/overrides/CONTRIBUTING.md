# Contributing to ClawFi

## Getting Started

```bash
git clone https://github.com/hxa-k/clawfi.git
cd clawfi
cd client && npm install && cd ..
cd server && npm install && cd ..
cp server/.env.example server/.env
npm run dev
```

## Branch Rules

- `main` is protected — no direct pushes
- All changes go through pull requests
- PRs require: CI passing + 1 approving review

## Workflow

1. Create a feature branch: `git checkout -b feat/your-feature main`
2. Make your changes
3. Run checks locally: `cd client && npm run build`
4. Push and open a PR
5. Address review feedback, then wait for merge

## Code Style

- TypeScript strict mode
- React functional components + hooks
- Tailwind CSS + shadcn/ui
- Express.js routes organized by feature
- Keep functions small, add comments for non-obvious logic

## Review Process

1. **PR proposer** opens a pull request
2. **Lisa** reviews for correctness, consistency, quality
3. Iterate if needed until Lisa approves
4. **Kevin** does final review and merges

## Reporting Issues

Open a GitHub issue with steps to reproduce, expected vs actual behavior, and screenshots.
