# Node.js & TypeScript

## Summary

Opinionated `Dockerfile` for Node.js and TypeScript development. This image includes a set of common tools and configurations for a smooth development experience.

- This image is built on top of the [Node.js](https://hub.docker.com/_/node) base image. Default image variant is `22-bookworm-slim`, which is based on the latest LTS version of Node.js (22.x) and Debian Bookworm.
	- [How to choose Node.js container image](https://labs.iximiuz.com/tutorials/how-to-choose-nodejs-container-image)
	- [Choosing the best Node.js Docker image](https://snyk.io/blog/choosing-the-best-node-js-docker-image/)
- Package Manager is set up through corepack, [PNPM](https://pnpm.io/) is enabled and configured by default.
- Common tools like `eslint`, `prettier`, and `typescript` are included.
- It includes `git`, `curl`, `zsh`, [`Oh My Zsh!`](https://ohmyz.sh/), a non-root `nonroot` user and a set of common dependencies for development.
- `Oh My Zsh!` common plugins like `z`, `zsh-completions`, `zsh-autosuggestions` and `zsh-syntax-highlighting` are included for a better shell experience.
- Non-root user (`nonroot`) with limited permissions for improved security.

| Argument | Description | Type | Default Value |
|----------|-------------|------|---------------|
| VARIANT | The image variant to use. See [supported tags](https://hub.docker.com/_/node) | `string` | `22-bookworm-slim` |

## Using this image

You can directly reference pre-built versions of `Dockerfile` by using the `image` property in `.devcontainer/devcontainer.json` or updating the `FROM` statement in your own  `Dockerfile` to one of the following. An example `Dockerfile` is included in this repository.

- `ghcr.io/6thpath/typescript-node`

Refer to [this guide](https://containers.dev/guide/dockerfile) for more details.

## License

This project is licensed under the [MIT License](https://github.com/6thpath/devcontainer-images/blob/main/LICENSE).
