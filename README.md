# Temporal IP Geolocation

A Temporal TypeScript workflow that fetches your public IP address and retrieves geolocation information.

## What It Does

1. **Fetches your public IP** using [icanhazip.com](https://icanhazip.com)
2. **Looks up geolocation** using [ip-api.com](http://ip-api.com)
3. **Returns a greeting** with your IP and location

Example output:
```
Hello, Alice. Your IP is 203.0.113.42 and your location is San Francisco, California, United States
```

## Project Structure

```
src/
├── activities.ts    # Activity implementations (API calls)
├── workflows.ts     # Workflow definition with retry policy
├── client.ts        # CLI client to trigger workflows
├── worker.ts        # Worker process
├── shared.ts        # Shared constants (task queue name)
└── mocha/
    ├── activities.test.ts        # Activity unit tests
    └── workflows-mocks.test.ts   # Workflow integration tests
```

## Activities

| Activity | Description | API |
|----------|-------------|-----|
| `getIP()` | Returns your public IP address | `https://icanhazip.com` |
| `getLocationInfo(ip)` | Returns city, region, country for an IP | `http://ip-api.com/json/{ip}` |

## Workflow

**`getAddressFromIP(name: string)`**

Orchestrates the activities with automatic retry policy:
- Initial retry interval: 1 second
- Maximum retry interval: 1 minute
- Backoff coefficient: 2x
- Activity timeout: 1 minute

## Prerequisites

- Node.js 22+ (see `.nvmrc`)
- [Temporal CLI](https://github.com/temporalio/cli/#installation)

## Running the Project

1. **Start Temporal Server** (in a separate terminal):
   ```bash
   temporal server start-dev
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Start the Worker** (in a separate terminal):
   ```bash
   npm run start.watch
   ```

4. **Run the Workflow**:
   ```bash
   npm run workflow YourName
   ```

## Available Scripts

| Script | Description |
|--------|-------------|
| `npm run start` | Start the worker |
| `npm run start.watch` | Start worker with hot-reload |
| `npm run workflow <name>` | Trigger the workflow |
| `npm test` | Run tests |
| `npm run build` | Compile TypeScript |
| `npm run lint` | Run ESLint |
| `npm run format` | Format code with Prettier |

## Testing

Run the test suite:
```bash
npm test
```

Tests include:
- **Activity unit tests** - Mock fetch calls to verify activity behavior
- **Workflow integration tests** - Mock activities to verify workflow orchestration
