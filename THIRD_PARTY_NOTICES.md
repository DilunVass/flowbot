# Third-party notices

Flowbot includes open source software. Each component is licensed under its own terms.

## Main components

| Component | License |
|---|---|
| Python | PSF License |
| FastAPI, Starlette | MIT |
| Uvicorn | BSD-3-Clause |
| Pydantic, pydantic-settings | MIT |
| llama.cpp, llama-cpp-python | MIT |
| NumPy | BSD-3-Clause |
| httpx | BSD-3-Clause |
| pypdf | BSD-3-Clause |
| cryptography | Apache-2.0 or BSD-3-Clause |
| bcrypt | Apache-2.0 |
| PyJWT | MIT |
| SlowAPI | MIT |
| python-dotenv | BSD-3-Clause |
| python-multipart | Apache-2.0 |
| email-validator | Unlicense |
| Vue, Vue Router, Pinia | MIT |
| crypto-js | MIT |

The full license text of every Python component ships inside the container image, in each package's `*.dist-info` folder under `/opt/venv/lib/python3.12/site-packages/`.

## Not part of Flowbot

The following are downloaded or run separately, and are covered by their own licenses:

- The model file you put in `models/` (Qwen3 is Apache 2.0)
- Ollama, if you use it
- The `python:3.12-slim` base image and its Debian packages
