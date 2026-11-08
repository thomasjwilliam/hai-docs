# HAI documentation

[Coming soon](https://thomasjwilliam.github.io/hai-docs/)...

## Technical

This website is built using [Docusaurus](https://docusaurus.io/), a modern static website generator.

### Installation

```
$ npm ci
```

This installs the exact versions pinned in `package-lock.json`. Use `npm install <package>` only when adding or upgrading a dependency.

### Local Development

```
$ npm start
```

This command starts a local development server and opens up a browser window. Most changes are reflected live without having to restart the server.

### Build

```
$ npm run build
```

This command generates static content into the `build` directory and can be served using any static contents hosting service.

### Deployment

The site is deployed to GitHub Pages by `.github/workflows/deploy.yml` whenever changes are pushed to `main`. There is no manual step.
