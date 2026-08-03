# IDURAR ERP CRM — Frontend

The React client for IDURAR ERP CRM. Built with **Vite 5**, **React 18**, **Ant Design 5** and **Redux Toolkit**, it talks to the Express API in [`../backend`](../backend) over `/api`.

## Requirements

- Node.js **20.9.0**
- npm **10.2.4**
- The backend running on `http://localhost:8888` (see the [root README](../README.md))

## Getting started

```bash
npm install
npm run dev
```

The app is served on [http://localhost:3000](http://localhost:3000). Vite proxies every `/api` request to the backend, so no CORS setup is needed in development. Edits hot-reload, and lint errors appear in the terminal.

## Available scripts

### `npm run dev`

Starts the Vite dev server on port 3000 with HMR, proxying `/api` to `http://localhost:8888`.

### `npm run dev:remote`

Same dev server, but proxies the API to `VITE_BACKEND_SERVER` instead of localhost. Use it to run the UI against a deployed backend.

### `npm run build`

Builds the production bundle into `dist/`. Assets are minified and filenames are hashed for cache-busting.

### `npm run preview`

Serves the contents of `dist/` locally so you can check a production build before deploying.

### `npm run lint`

Runs ESLint across `.js` / `.jsx` sources with `--max-warnings 0`, so any warning fails the run. Formatting is handled by Prettier (`.prettierrc`).

## Environment variables

Create a `.env` in this directory (`temp.env` is the template). Vite only exposes variables prefixed with `VITE_`.

| Variable              | Description                                                             |
| --------------------- | ----------------------------------------------------------------------- |
| `VITE_BACKEND_SERVER` | Backend base URL, used in production builds and by `npm run dev:remote` |
| `VITE_FILE_BASE_URL`  | Base URL for uploaded files and images                                  |
| `PROD`                | Set to `true` for production builds                                     |

All API endpoints are derived from these in [src/config/serverApiConfig.js](src/config/serverApiConfig.js).

## Project structure

```
src/
  apps/         IdurarOs + ErpApp shells, header and navigation
  auth/         auth service and session handling
  components/   shared UI — DataTable, CrudModal, SelectAsync, SidePanel…
  config/       API base URLs and auth token name
  context/      app, crud, erp and profile context providers
  forms/        entity forms (Customer, Payment, Tax, Currency, Admin…)
  hooks/        useFetch, useOnFetch, useDebounce, useResponsive, useMail…
  layout/       Auth, Crud, Dashboard, Erp, Profile and Settings layouts
  locale/       localization provider, antd locales, translations
  modules/      feature modules — Crud, ErpPanel, Invoice, Payment, Quote,
                Dashboard, Auth, Profile, Setting
  pages/        routed pages — Invoice, Payment, Customer, Settings, auth
  redux/        Redux Toolkit store, slices (auth, crud, erp, settings), persistence
  request/      axios instance, error/success handlers, status messages
  router/       AppRouter, AuthRouter and route definitions
  settings/     app settings hooks (useDate, useMoney)
  style/        global styles, theme partials and images
  utils/        calculations, currency/country lists, tag colors, helpers
```

`@` is aliased to `src/`, so imports look like `import { erp } from '@/redux/erp/actions'`.

## Notes

- State that must survive a reload (auth session, settings) is persisted via `src/redux/storePersist.js`.
- All HTTP calls go through `src/request/request.js`, which attaches credentials and funnels errors into a single notification handler.
- Adding a new entity screen usually means: a module under `src/modules`, a form in `src/forms`, a page in `src/pages`, and a route in `src/router/routes.jsx`.
- Money and date formatting follow the company settings loaded from the API — use `useMoney()` and `useDate()` from `src/settings` rather than formatting inline.

## License

GNU Affero General Public License v3.0 — see [LICENSE](../LICENSE).
