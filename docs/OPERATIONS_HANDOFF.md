# Operations Handoff: Deployment & Maintenance

## 1. Deployment Readiness
The project is structurally ready for deployment on platforms like Render, Railway, or Vercel (frontend) + DigitalOcean (backend).

## 2. Environment Configuration
The following environment variables are required for production:
- `PORT`: Backend server listener port (defaults to 3001).
- `VITE_API_BASE_URL`: The public-facing endpoint of the Backend API for the Frontend to consume.

## 3. Deployment Checklist
1. [ ] Abstraction of `API_URL` to `.env`.
2. [ ] Configure Production CORS policy.
3. [ ] Run `npm run build` for the React production bundle.
4. [ ] Set up a recurring backup job for `backend/tasks.db`.

## 4. Maintenance Notes
- **Persistence**: Data is stored in `backend/tasks.db`. This file must be persisted across deployments (e.g., using Docker Volumes or Persistent Disk).
- **Scaling**: For high-concurrency needs, migration to PostgreSQL is recommended.
