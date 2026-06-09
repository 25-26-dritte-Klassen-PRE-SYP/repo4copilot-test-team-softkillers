# 1. Tech Stack

| Bereich            | Technologie                | Begründung                                                   |
| ------------------ | -------------------------- | ------------------------------------------------------------ |
| Frontend & Backend | Next.js 15 (App Router)    | Frontend, API und Server Components in einer Anwendung       |
| Sprache            | TypeScript                 | Typsicherheit über den gesamten Stack                        |
| UI                 | Tailwind CSS + shadcn/ui   | Schnelle Entwicklung einer konsistenten Benutzeroberfläche   |
| State Management   | React Query                | Datenabruf, Caching und Synchronisierung mit dem Backend     |
| Authentifizierung  | Auth.js                    | Session- und Benutzerverwaltung                              |
| Datenbank          | PostgreSQL                 | Relationale Datenhaltung für Nutzer-, Preis- und Listendaten |
| ORM                | Prisma                     | Typsicherer Datenbankzugriff und Migrationen                 |
| Karten & Routing   | Google Maps JavaScript API | Kartenanzeige, Geocoding und Routenberechnung                |
| Hintergrundjobs    | Vercel Cron Jobs           | Zeitgesteuerte Preisabfragen und Alert-Prüfungen             |
| Hosting            | Vercel                     | Deployment von Frontend und API                              |
| Monitoring         | Sentry                     | Fehlererfassung und Monitoring                               |
| CI/CD              | GitHub Actions + Vercel    | Automatisierte Tests und Deployments                         |
| API-Validierung    | Zod                        | Typisierte Request- und Response-Validierung                 |

## Infrastruktur

| Komponente         | Technologie                  | Begründung                                   |
| ------------------ | ---------------------------- | -------------------------------------------- |
| Hosting            | Vercel                       | Serverless Deployment für Web-App und API    |
| Datenbank          | Neon PostgreSQL              | Managed PostgreSQL mit automatischen Backups |
| Secrets Management | Vercel Environment Variables | Verwaltung von API-Keys und Zugangsdaten     |
| Logging            | Vercel Logs                  | Zentrale Server- und API-Logs                |