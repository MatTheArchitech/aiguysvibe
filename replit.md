# Cloudflare VibeSDK - Replit Environment

## Project Overview

This is the **Cloudflare VibeSDK** - an open source full-stack AI webapp generator built on Cloudflare's developer platform. This project allows users to build applications using natural language prompts, with AI generating and deploying the applications.

**Live Demo**: [build.cloudflare.dev](https://build.cloudflare.dev)

## Architecture

### Stack
- **Frontend**: React 19 + Vite + TypeScript + Tailwind CSS
- **Backend**: Cloudflare Workers with Hono framework
- **Database**: Cloudflare D1 (SQLite) with Drizzle ORM
- **Build Tool**: Bun (package manager) + Vite (bundler using Rolldown)
- **Deployment**: Cloudflare Workers for Platforms

### Key Technologies
- Durable Objects for stateful AI agents
- Cloudflare Containers for sandboxed app previews
- R2 buckets for templates storage
- KV for session management
- Workers Dispatcher for deploying generated apps

## Replit Setup (Completed)

### Changes Made for Replit Compatibility

1. **Dependencies Installation**
   - Uses Bun for package management (faster than npm)
   - All dependencies installed successfully

2. **Environment Configuration**
   - Created `.dev.vars` file with essential environment variables
   - Required secrets: `GOOGLE_AI_STUDIO_API_KEY`, `JWT_SECRET`, `WEBHOOK_SECRET`, `SECRETS_ENCRYPTION_KEY`
   - Optional secrets for OAuth and additional AI providers

3. **Vite Configuration Updates**
   - Configured server to bind to `0.0.0.0:5000` for Replit proxy
   - Enabled `allowedHosts: true` for Replit's iframe proxy
   - Added polling for file watching (prevents ENOSPC errors)
   - **Disabled Cloudflare Vite plugin** for local frontend development (containers not available in Replit)

4. **Wrangler Configuration**
   - Disabled containers in dev mode (`dev.enable_containers: false`)
   - Local D1 database migrations applied successfully

5. **Package.json Updates**
   - Updated `dev` script to bind to correct host/port: `vite --host 0.0.0.0 --port 5000`

## Development Workflow

### Running Locally (Replit)

The frontend development server is configured as a workflow:
- **Command**: `bun run dev`
- **Port**: 5000
- **Status**: Running ✅

### Frontend-Only Development

Currently configured for **frontend development only** in Replit:
- Cloudflare Workers plugin is disabled
- No backend API calls will work (would need Cloudflare Workers environment)
- Frontend UI and components can be developed and tested

### Full-Stack Development

For full Cloudflare Workers development (requires Cloudflare account):
1. Uncomment the Cloudflare plugin in `vite.config.ts`
2. Add all required environment variables to `.dev.vars`
3. Use `bun run dev:worker` for local worker development with wrangler

## Database

### Local D1 Database
- Migrations located in `migrations/` directory
- Local database stored in `.wrangler/state/v3/d1`
- Database name: `vibesdk-db`

### Running Migrations
```bash
bun run db:migrate:local     # Apply migrations locally
bun run db:studio            # Open Drizzle Studio UI
bun run db:generate          # Generate new migrations
```

## Important Files

### Configuration
- `wrangler.jsonc` - Cloudflare Workers configuration
- `vite.config.ts` - Vite bundler configuration (modified for Replit)
- `tsconfig.json` - TypeScript configuration
- `.dev.vars` - Local environment variables (not committed)
- `drizzle.config.local.ts` - Database configuration for local dev

### Entry Points
- `src/main.tsx` - Frontend React application entry
- `worker/index.ts` - Cloudflare Workers entry (backend)
- `src/routes.ts` - React Router configuration

### Key Directories
- `src/` - Frontend React application code
- `worker/` - Backend Cloudflare Workers code
- `shared/` - Shared types and utilities
- `migrations/` - D1 database migrations
- `public/` - Static assets

## Known Limitations in Replit

1. **No Backend API**: Cloudflare Workers backend is not running in Replit (requires Cloudflare infrastructure)
2. **No Containers**: Cloudflare Containers for app sandboxing not available
3. **No Durable Objects**: Stateful AI agents require Cloudflare Workers environment
4. **Frontend Only**: Can develop and preview UI components only

## Deployment

### To Cloudflare Workers

For production deployment to Cloudflare:
1. Create `.prod.vars` file with production secrets
2. Configure Cloudflare account in wrangler.jsonc
3. Run: `bun run deploy`

This requires:
- Cloudflare Workers Paid Plan
- Workers for Platforms subscription
- Advanced Certificate Manager (for wildcard preview domains)

### Production Environment Variables

Required for deployment:
- `GOOGLE_AI_STUDIO_API_KEY` - Google Gemini API key
- `JWT_SECRET` - Secure random string for sessions
- `WEBHOOK_SECRET` - Webhook authentication
- `SECRETS_ENCRYPTION_KEY` - Encryption key for secrets
- `CUSTOM_DOMAIN` - Your custom domain
- `ALLOWED_EMAIL` - Authorized user email

Optional OAuth:
- Google: `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`
- GitHub: `GITHUB_CLIENT_ID`, `GITHUB_CLIENT_SECRET`
- GitHub Export: `GITHUB_EXPORTER_CLIENT_ID`, `GITHUB_EXPORTER_CLIENT_SECRET`

## Scripts Reference

### Development
- `bun run dev` - Start frontend dev server on port 5000
- `bun run dev:worker` - Start with Cloudflare Workers (requires wrangler)
- `bun run build` - Build for production

### Database
- `bun run db:migrate:local` - Run migrations locally
- `bun run db:migrate:remote` - Run migrations on remote D1
- `bun run db:studio` - Open Drizzle Studio
- `bun run db:push:local` - Push schema changes locally

### Code Quality
- `bun run lint` - Run ESLint
- `bun run test` - Run tests with Vitest

### Deployment
- `bun run deploy` - Deploy to Cloudflare Workers

## Resources

- [Cloudflare VibeSDK GitHub](https://github.com/cloudflare/vibesdk)
- [Cloudflare Workers Docs](https://developers.cloudflare.com/workers/)
- [Cloudflare D1 Database](https://developers.cloudflare.com/d1/)
- [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/)

## Recent Changes (Replit Setup - Sep 29, 2025)

1. Installed dependencies using Bun
2. Created `.dev.vars` with development environment variables
3. Configured Vite for Replit (host, port, polling, allowedHosts)
4. Disabled Cloudflare plugin for frontend-only development
5. Applied local D1 database migrations
6. Set up workflow for frontend dev server on port 5000

## Next Steps

For users wanting to extend this project:

1. **Add API Keys**: Update `.dev.vars` with your Google AI Studio API key to enable AI features
2. **Full Workers Dev**: Enable Cloudflare plugin and configure wrangler for full-stack development
3. **Deploy**: Follow deployment guide to publish to Cloudflare Workers

## Support

- For Cloudflare VibeSDK issues: [GitHub Issues](https://github.com/cloudflare/vibesdk/issues)
- For Cloudflare Workers support: [Cloudflare Discord](https://discord.gg/cloudflaredev)
