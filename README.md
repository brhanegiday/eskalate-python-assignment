# Eskalate Python Assignment

A small Python HTTP client with OAuth2 token management. The project demonstrates token-based authentication for API requests, including automatic token refresh when tokens are missing, expired, or invalid.

## Project Structure

```
├── app/
│   ├── __init__.py
│   ├── http_client.py    # HTTP client with OAuth2 auth support
│   └── tokens.py         # OAuth2Token dataclass and helpers
├── tests/
│   ├── conftest.py       # Test configuration
│   └── test_http_client.py  # Test suite
├── Dockerfile            # CI-style test runner
├── Explanation.md        # Bug analysis and fix rationale
├── requirements.txt      # Pinned dependencies
└── README.md
```

## How to Run Tests Locally

1. **Install dependencies:**

   ```bash
   pip install -r requirements.txt
   ```

2. **Run the test suite:**

   ```bash
   pytest -v
   ```

## How to Build and Run Tests with Docker

1. **Build the Docker image:**

   ```bash
   docker build -t eskalate-python-assignment .
   ```

2. **Run the tests:**

   ```bash
   docker run --rm eskalate-python-assignment
   ```
