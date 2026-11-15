# Octopus Deploy OpenFeature Provider for TypeScript/JavaScript (web clients)

[![npm](https://img.shields.io/npm/v/@octopusdeploy/openfeature)](https://www.npmjs.com/package/@octopusdeploy/openfeature)
[![CI build](https://github.com/OctopusDeploy/openfeature-provider-ts/actions/workflows/main.yml/badge.svg?branch=main)](https://github.com/OctopusDeploy/openfeature-provider-ts/actions/workflows/main.yml?query=branch:main)

The Octopus Deploy TypeScript/JavaScript [OpenFeature provider](https://openfeature.dev/docs/reference/concepts/provider/) for web clients, to be used with the [OpenFeature web SDK](https://openfeature.dev/docs/reference/technologies/client/web/).

For Node, use [`@octopusdeploy/openfeature-server`](https://www.npmjs.com/package/@octopusdeploy/openfeature-server) instead.

## Installation

```
npm i @octopusdeploy/openfeature @openfeature/web-sdk
```

`@openfeature/web-sdk` is a peer dependency, so your app and the provider share a single copy of the OpenFeature SDK. npm 7+ and pnpm 8+ install it automatically. Yarn does not, so list `@openfeature/web-sdk` and its own peer dependency `@openfeature/core` in your `package.json`.

## Usage

```ts
import { OctopusFeatureProvider, ProductMetadata } from "@octopusdeploy/openfeature";
import { OpenFeature } from "@openfeature/web-sdk";

const provider = new OctopusFeatureProvider({
    clientIdentifier: "YourClientIdentifier",
    productMetadata: new ProductMetadata("YourProductName", "1.0.0"),
});

await OpenFeature.setContext({ userid: "bob@octopus.com" });
try {
    await OpenFeature.setProviderAndWait(provider);
} catch (error) {
    console.error("Failed to initialize provider:", error);
}

const client = OpenFeature.getClient();

if (client.getBooleanValue("to-the-moon-feature", false, {})) {
    console.log("🚀🚀🚀");
}
```

Octopus feature flags are boolean, so evaluate them with `getBooleanValue`. `getStringValue`, `getNumberValue` and `getObjectValue` return your default value with a `TYPE_MISMATCH` error code.

The provider fetches flags when it initializes and saves them in the browser's `localStorage`. If it cannot retrieve them, it falls back to the flags it saved on an earlier page load and logs a warning that they may be stale. If there are none, `setProviderAndWait` rejects; catch the rejection if your application should start anyway, and every evaluation will return your default value.

The provider fetches flags once per initialization, usually once per page load, and doesn't poll for changes. Changing the evaluation context with `OpenFeature.setContext` evaluates the flags it already has against the new context without fetching them again.

## Configuration

| Option | Required | Description |
| --- | --- | --- |
| `clientIdentifier` | Yes | The client identifier from the Octopus variable `Octopus.FeatureFlags.ClientIdentifier`. |
| `productMetadata` | Yes | The name and, optionally, version of your application, sent to Octopus for telemetry. |
| `releaseVersionOverride` | No | Overrides the release version embedded in the client identifier. |
| `serverUri` | No | The Octopus Feature Flags server to fetch flags from. Defaults to `https://features.octopus.com`. |
| `logger` | No | An OpenFeature `Logger` for the provider's own log messages. |

## About Octopus Deploy

[Octopus Deploy](https://octopus.com) is a sophisticated, best-of-breed continuous delivery (CD) platform for modern software teams. Octopus offers powerful release orchestration, deployment automation, and runbook automation, while handling the scale, complexity and governance expectations of even the largest organizations with the most complex deployment challenges.
