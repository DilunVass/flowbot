# Third-party notices

Flowbot includes open source software. Each component is licensed under its own terms, reproduced in full in the notice files attached to every release.

For each version, see the release assets:

- `THIRD_PARTY_NOTICES.txt`: backend (Python) components and their licenses
- `THIRD_PARTY_NOTICES_JS.txt`: dashboard and widget (JavaScript) components and their licenses

The same files are included inside the container image at `/opt/flowbot/licenses/`.

Container images that run alongside Flowbot and are pulled from their own publishers (for example `pgvector/pgvector` and `ollama/ollama`) are not part of Flowbot and are governed by their own licenses.
