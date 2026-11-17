# Octopus Deploy OpenFeature Providers for TypeScript/JavaScript

[![CI build](https://github.com/OctopusDeploy/openfeature-provider-ts/actions/workflows/main.yml/badge.svg?branch=main)](https://github.com/OctopusDeploy/openfeature-provider-ts/actions/workflows/main.yml?query=branch:main)

[OpenFeature providers](https://openfeature.dev/docs/reference/concepts/provider/) for Octopus Deploy Feature Flags. Pick the package that matches the OpenFeature SDK your application uses:

| Package | Runs in | OpenFeature SDK | Docs |
| --- | --- | --- | --- |
| [![npm](https://img.shields.io/npm/v/@octopusdeploy/openfeature)](https://www.npmjs.com/package/@octopusdeploy/openfeature) `@octopusdeploy/openfeature` | Browsers | [`@openfeature/web-sdk`](https://openfeature.dev/docs/reference/technologies/client/web/) | [README](packages/web/README.md) |
| [![npm](https://img.shields.io/npm/v/@octopusdeploy/openfeature-server)](https://www.npmjs.com/package/@octopusdeploy/openfeature-server) `@octopusdeploy/openfeature-server` | Node 22+ | [`@openfeature/server-sdk`](https://openfeature.dev/docs/reference/technologies/server/javascript/) | [README](packages/server/README.md) |

Each package's README covers installation and usage, and is the same README npm shows for that package.

Both providers depend on [`@octopusdeploy/openfeature-core`](packages/core/README.md), which holds the logic they share. It's internal, so don't install it directly.

## About Octopus Deploy

[Octopus Deploy](https://octopus.com) is a sophisticated, best-of-breed continuous delivery (CD) platform for modern software teams. Octopus offers powerful release orchestration, deployment automation, and runbook automation, while handling the scale, complexity and governance expectations of even the largest organizations with the most complex deployment challenges.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) to build and test locally, and [RELEASING.md](RELEASING.md) for how the packages are released.

## License

[Apache 2.0](LICENSE)
