# Octopus Deploy OpenFeature Provider for Node

[![npm](https://img.shields.io/npm/v/@octopusdeploy/openfeature-server)](https://www.npmjs.com/package/@octopusdeploy/openfeature-server)
[![CI build](https://github.com/OctopusDeploy/openfeature-provider-ts/actions/workflows/main.yml/badge.svg?branch=main)](https://github.com/OctopusDeploy/openfeature-provider-ts/actions/workflows/main.yml?query=branch:main)

The Octopus Deploy TypeScript/JavaScript [OpenFeature provider](https://openfeature.dev/docs/reference/concepts/provider/) for Node, to be used with the [OpenFeature server SDK](https://openfeature.dev/docs/reference/technologies/server/javascript/). It requires Node 22 or later.

For browsers, use [`@octopusdeploy/openfeature`](https://www.npmjs.com/package/@octopusdeploy/openfeature) instead.

## Installation

```
npm i @octopusdeploy/openfeature-server @openfeature/server-sdk
```

`@openfeature/server-sdk` is a peer dependency, so your app and the provider share a single copy of the OpenFeature SDK. npm 7+ and pnpm 8+ install it automatically. Yarn does not, so list `@openfeature/server-sdk` and its own peer dependency `@openfeature/core` in your `package.json`.

## Usage

```ts
import { OctopusFeatureProvider, ProductMetadata } from "@octopusdeploy/openfeature-server";
import { OpenFeature } from "@openfeature/server-sdk";

const provider = new OctopusFeatureProvider({
    clientIdentifier: "YourClientIdentifier",
    productMetadata: new ProductMetadata("YourProductName", "1.0.0"),
});

await OpenFeature.setProviderAndWait(provider);

const client = OpenFeature.getClient();

// Evaluation context is passed with each evaluation, so one provider serves every request.
if (await client.getBooleanValue("to-the-moon-feature", false, { userid: "bob@octopus.com" })) {
    console.log("🚀🚀🚀");
}
```

Octopus feature flags are boolean, so evaluate them with `getBooleanValue`. `getStringValue`, `getNumberValue` and `getObjectValue` return your default value with a `TYPE_MISMATCH` error code.

The provider fetches flags when it initializes. If it cannot retrieve them, `setProviderAndWait` rejects and every evaluation returns your default value.

After that, it checks for changes every minute and fetches the flags again only when they have changed. Set `cacheDurationMs` to check more or less often. If a check fails, the provider keeps evaluating against the flags it already has and tries again at the next interval. If initialization failed, it keeps trying and moves to `READY` once the flags can be fetched.

Polling stops when the provider is closed, for example by `OpenFeature.close()`. The polling timer never keeps the Node process alive on its own.

## Configuration

| Option | Required | Description |
| --- | --- | --- |
| `clientIdentifier` | Yes | The client identifier from the Octopus variable `Octopus.FeatureFlags.ClientIdentifier`. |
| `productMetadata` | Yes | The name and, optionally, version of your application, sent to Octopus for telemetry. |
| `cacheDurationMs` | No | How long, in milliseconds, between checks for changed flags. Defaults to one minute. Must be greater than 0. |
| `releaseVersionOverride` | No | Overrides the release version embedded in the client identifier. |
| `serverUri` | No | The Octopus Feature Flags server to fetch flags from. Defaults to `https://features.octopus.com`. |
| `logger` | No | An OpenFeature `Logger` for the provider's own log messages. |

## About Octopus Deploy

[Octopus Deploy](https://octopus.com) is a sophisticated, best-of-breed continuous delivery (CD) platform for modern software teams. Octopus offers powerful release orchestration, deployment automation, and runbook automation, while handling the scale, complexity and governance expectations of even the largest organizations with the most complex deployment challenges.
