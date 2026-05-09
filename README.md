# React Module Federation Example

This project is now structured as a real micro-frontend example with three separate apps:

- `host/` - shell application that renders the page and consumes remote modules
- `app1/` - remote application exposing a product catalog widget
- `app2/` - remote application exposing a team/profile widget

## Why This Matches Real Micro Frontends Better

You mentioned the mental model correctly: in practice, micro frontends are often separate projects that come together in one UI. This repo now reflects that idea directly.

Instead of one `src/` pretending to have multiple apps, you now have:

```text
host/
app1/
app2/
```

Each app has its own:

- `src/`
- `webpack.config.js`
- entry point
- UI

The host loads remote modules from `app1` and `app2` using Webpack Module Federation.

## Ports

- Host: `http://localhost:3000`
- App 1 remote: `http://localhost:3001`
- App 2 remote: `http://localhost:3002`

## Run The Apps

Open three terminals in the project root and run:

```bash
npm run start:app1
```

```bash
npm run start:app2
```

```bash
npm run start:host
```

Then open `http://localhost:3000`.

## Build

```bash
npm run build
```

This builds all three apps in this order:

1. `app1`
2. `app2`
3. `host`

## What To Look At

### Host

- [host/webpack.config.js](/Users/gauravyadav/Desktop/react-redux-thunk-demo/host/webpack.config.js:1)
- [host/src/App.js](/Users/gauravyadav/Desktop/react-redux-thunk-demo/host/src/App.js:1)

The host defines remotes:

- `app1@http://localhost:3001/remoteEntry.js`
- `app2@http://localhost:3002/remoteEntry.js`

and renders components exposed by those remote apps.

### App 1

- [app1/webpack.config.js](/Users/gauravyadav/Desktop/react-redux-thunk-demo/app1/webpack.config.js:1)
- [app1/src/ProductCatalog.js](/Users/gauravyadav/Desktop/react-redux-thunk-demo/app1/src/ProductCatalog.js:1)

This remote exposes:

- `./ProductCatalog`

### App 2

- [app2/webpack.config.js](/Users/gauravyadav/Desktop/react-redux-thunk-demo/app2/webpack.config.js:1)
- [app2/src/TeamProfile.js](/Users/gauravyadav/Desktop/react-redux-thunk-demo/app2/src/TeamProfile.js:1)

This remote exposes:

- `./TeamProfile`

## Module Federation Flow

1. `app1` and `app2` run on their own ports and publish `remoteEntry.js`
2. `host` declares those remotes in its webpack config
3. `host/src/App.js` imports remote modules using:
   - `import("app1/ProductCatalog")`
   - `import("app2/TeamProfile")`
4. The host renders them into one page

## Legacy Folder

The old `src/` folder is still in the repo from the earlier single-app learning demo, but the active micro-frontend example now lives in:

- `host/`
- `app1/`
- `app2/
