# Security Notes for Repository Maintainers

- One previously uploaded Shuffle configuration screenshot visibly exposed a bearer credential. That screenshot is intentionally excluded from this repository. If the credential remains active, revoke/rotate it.
- A historical AWS security-group screenshot showed TCP/9000 open to `0.0.0.0/0`. Verify the current TheHive security-group configuration and restrict access to trusted sources.
- TheHive API was tested over HTTP. Use HTTPS before any non-lab deployment.
- Never publish raw Shuffle exports until webhook URLs, API keys, authorization headers, email addresses, and private endpoints have been reviewed and sanitized.
