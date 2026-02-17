# Explanation

## What was the bug?

When `oauth2_token` was set to a **dict** (e.g. `{"access_token": "stale", "expires_at": 0}`), the `Client.request()` method failed to refresh the token and did not set the `Authorization` header.

## Why did it happen?

The refresh condition in `http_client.py` only checked two cases:

1. `not self.oauth2_token` — catches `None` or empty/falsy values.
2. `isinstance(self.oauth2_token, OAuth2Token) and self.oauth2_token.expired` — catches expired `OAuth2Token` instances.

A non-empty **dict** is truthy, so check 1 evaluates to `False`. It is also not an `OAuth2Token` instance, so check 2 evaluates to `False`. The result: the token was neither refreshed nor used, and the request was sent without any `Authorization` header.

## Why does your fix solve it?

The fix adds a third condition:

```python
or (not isinstance(self.oauth2_token, OAuth2Token))
```

This ensures that any token value that is not a proper `OAuth2Token` instance (such as a raw dict) triggers a refresh, producing a valid `OAuth2Token` with a fresh access token.

## One realistic edge case the tests still don't cover

The tests do not cover the scenario where `oauth2_token` is an **empty dict** (`{}`). An empty dict is falsy, so `not self.oauth2_token` would be `True` and the token would be refreshed — which happens to be correct behavior. However, this edge case is not explicitly tested, and if the truthiness behavior of the token value ever changed (e.g. through a custom `__bool__`), the logic could silently break.
