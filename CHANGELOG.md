# Changelog

All notable changes to Flowbot are listed here. Versions follow [semantic versioning](https://semver.org): breaking changes increase the first number.

Before upgrading, read the **Upgrade notes** for every version between yours and the new one.

## [v1.1.0] - 2026-10-01

### Added
- Email through Gmail: project invitations and handoff notices are now sent by email. Earlier versions created them but sent nothing
- Edit a chatbot's composed prompt by hand in the Prompt tab. A hand-edited prompt is used as written until you save new choices or rebuild
- Install guides for macOS and Windows, alongside Linux
- Backup and restore commands for Windows (PowerShell)

### Changed
- The docs now state that OpenAI is the only supported provider for the knowledge tier. Support for other providers is planned

### Upgrade notes
- No changes are required.
- To turn on email, add `GMAIL_USER` and `APP_PASSWORD` to `.env` and run `docker compose up -d`. See [configuration.md](docs/configuration.md#email). Without them, invited teammates don't receive their invitation link.

## [v1.0.1] - 2026-09-30

### Changed
- The public site's header and footer link only to pages that exist

### Upgrade notes
- None.

## [v1.0.0] - 2026-09-29

First public release.

### Added
- Flow builder with menus, buttons and example phrasings
- Three chatbot modes: flows only, documents only, and hybrid
- Local routing model, run in-process from a GGUF file or through an OpenAI-compatible server such as Ollama
- Knowledge base from PDF, TXT, Markdown and HTML files, web pages and pasted text, answered with OpenAI
- Suggested flow nodes for questions the knowledge base keeps answering
- Handoff queue for conversations the bot can't answer
- Projects with team members, API keys, and usage limits per project and chatbot
- Usage statistics by answer tier
- Single SQLite database with no separate database server

### Upgrade notes
- None (first release).
