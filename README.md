# Dev Events — Next.js 16 Event Platform

Discover and share developer events, from hackathons and meetups to conferences. Built with Next.js 16 (App Router), TypeScript, MongoDB and Tailwind CSS.

[**Live demo**](https://dev-events-next-js-16-six.vercel.app/) · [**Source code**](https://github.com/a-basuony/Dev_Events) · [**Report an issue**](https://github.com/a-basuony/Dev_Events/issues)

![Dev Events application preview](public/readme/readme-hero.webp)

## Features

- Browse events and open detailed event pages using readable URLs.
- Create events with an image, agenda and tags.
- Register interest in an event using the booking flow.
- Persist events and bookings with MongoDB and Mongoose.
- Upload event images through Cloudinary.
- Responsive UI and server-side event rendering.

> **Project status:** This is a learning/portfolio project. The existing event-management endpoints should not be exposed for unrestricted production use without implementing authentication, authorization, input validation and rate limiting.

## Tech stack

| Layer | Technology |
| --- | --- |
| App | Next.js 16 App Router, React 19, TypeScript |
| UI | Tailwind CSS 4 |
| Database | MongoDB + Mongoose |
| Image uploads | Cloudinary |
| Observability | PostHog |
| Hosting | Vercel |

## Quick start

**Requirements:** Node.js compatible with Next.js 16 (20.9+ recommended), npm, and access to MongoDB.

```bash
git clone https://github.com/a-basuony/Dev_Events.git
cd Dev_Events
npm ci
```

Create a local `.env.local` file (do not commit it):

```dotenv
MONGODB_URI=mongodb://127.0.0.1:27017/dev-events
NEXT_PUBLIC_BASE_URL=http://localhost:3000

# Required to upload event images: configure Cloudinary credentials
CLOUDINARY_URL=cloudinary://api_key:api_secret@cloud_name

# Analytics; configure a valid public PostHog project key if enabled
NEXT_PUBLIC_POSTHOG_KEY=your_posthog_project_key
```

For a remote database, replace `MONGODB_URI` with the connection URI supplied by your MongoDB provider. For Cloudinary, configure credentials using the format supported by your Cloudinary Node SDK/version. Keep the Cloudinary API secret server-side, never in `NEXT_PUBLIC_*` variables. If you do not need image uploads, you can explore existing events without creating new ones.

Start the development server:

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

### Scripts

| Command | Purpose |
| --- | --- |
| `npm run dev` | Start local development server |
| `npm run build` | Build for production |
| `npm run start` | Start the production server after building |
| `npm run lint` | Run ESLint |

## Routes

| Path | Behavior |
| --- | --- |
| `/` | Home / featured events |
| `/events` | Browse events |
| `/events/[slug]` | Event details |
| `/events/create` | Event creation form |
| `GET /api/events` | List events |
| `POST /api/events` | Create an event (multipart form data) |
| `GET /api/events/[slug]` | Fetch an event by slug |
| `DELETE /api/events?id=<id>` | Delete an event |

The API currently also contains an update handler. Review the actual method and URL semantics in `app/api/events/route.ts` before integrating external clients. Protect write endpoints before using this project in a public production environment.

## Project structure

```text
app/                  Next.js pages, layouts and API handlers
components/           Reusable UI components and event forms
database/             Mongoose event and booking models
lib/actions/          Server actions for booking and related events
lib/mongodb.ts        Cached MongoDB connection helper
public/readme/        README images
```

## Troubleshooting

- **Clone fails / repository not found:** Use the `a-basuony/Dev_Events` URL above, not the outdated repository name from earlier documentation.
- **Database configuration error:** Set `MONGODB_URI` in `.env.local`, ensure MongoDB is reachable, then restart the dev server.
- **No events appear locally:** The app uses `NEXT_PUBLIC_BASE_URL` for server-side fetching of its events API. Set it to your local origin for local development, and make sure MongoDB contains event documents.
- **Image upload fails:** Check Cloudinary credentials and the server-side upload configuration.
- **Analytics error:** Configure `NEXT_PUBLIC_POSTHOG_KEY` for your PostHog project. This key is a public client-side project key, not a private API token.

## Contributing

Issues describing reproducible bugs, accessibility problems or improvements are welcome. For code changes, open a focused pull request with a short explanation and how you verified it. Never include real `.env` credentials or customer data.

## License

No license file is currently provided. The repository being public does **not** grant permission to reuse or redistribute its code under an open-source license. Please contact the repository owner before reusing the code.
