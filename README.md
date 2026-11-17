# spendwise-notification-service

Event-driven alert dispatcher: consumes `transaction-events` (Avro, Schema Registry), records each alert in `notification_db` exactly once (`processed_events` idempotency
guard), and has no public business API.

| | |
|---|---|
| Port | 8085 |
| Database | `notification_db` (Flyway migrations in `src/main/resources/db/migration`) |
| Consumes | `transaction-events` (consumer group `notification-service`) |
| Configuration | `notification-service*.yml` in [spendwise-config](https://github.com/PrathameshMax/spendwise-config) |
| Shared code | `com.spendwise:spendwise-parent / spendwise-common / spendwise-events` from [spendwise-shared](https://github.com/PrathameshMax/spendwise-shared) |
| Image | `ghcr.io/prathameshmax/spendwise-notification-service:<git-sha>` and `:main` |

## Build

Reading the shared packages needs a GitHub token with `read:packages` in `GITHUB_PACKAGES_TOKEN`
and a `<server>` with id `github` in `~/.m2/settings.xml`.

```bash
./mvnw -B verify
docker build --secret id=gh_token,env=GITHUB_PACKAGES_TOKEN -t spendwise-notification-service .
```

## CI

`.github/workflows/ci.yml`: every pull request builds, tests and builds the image; every push to
`main` also pushes the image to GHCR, tagged with the commit SHA and `main`.
