# Changelog

All notable changes to Flowbot are listed here. Versions follow [semantic versioning](https://semver.org): breaking changes increase the first number.

Before upgrading, read the **Upgrade notes** for every version between yours and the new one.

## [v1.0.0] - 2026-XX-XX

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
