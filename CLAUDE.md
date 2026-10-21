# CLAUDE.md

A small Express REST API (users + health check) used as the starter project for a Claude Code course. The app code must not be changed unless a task says so.

## Commands

```
npm install
npm run dev                                      # start with auto-reload on http://localhost:3000 (PORT overrides)
npm test                                         # node's built-in runner (node --test), finds tests/*.test.js
node --test tests/users.test.js                  # run one test file
node --test --test-name-pattern="404"            # run only tests whose name matches
npm run lint                                     # eslint . (eslint 8, .eslintrc.json)
```

CI (`.github/workflows/ci.yml`) runs `npm run lint` then `npm test` on Node 22 for every push and PR, so both must pass.

## Conventions

- Use CommonJS (`require` / `module.exports`), not ES modules. ESLint is configured with `sourceType: "script"`, so `import` fails lint.
- Errors are JSON `{ error: "<message>" }` with the matching status (400 for invalid input, 404 for missing records). Successful creates return 201 with the created object.
- Tests use `node:test` + `node:assert` + `supertest` against the exported `app`. Don't add Jest or Mocha, and don't call `app.listen` in tests.

## Architecture

- `server.js` builds the Express app, mounts one router per resource (`/users`, `/health`), and exports `app`. It only calls `listen` when run directly (`require.main === module`), which is what lets tests import it without opening a port.
- `routes/<resource>.js` holds one `express.Router()` per resource. A new resource means a new file here plus an `app.use(...)` line in `server.js`.
- `db/store.js` is an in-memory store standing in for a database. State resets on every restart and is shared across tests within one process (e.g. a `POST /users` in one test is visible to later tests).
- Routes never touch data directly. They call functions exported from `db/store.js`; add a new store function rather than reaching into its arrays.
