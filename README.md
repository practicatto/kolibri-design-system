
# Kolibri Design System

## Usage

How to use the Kolibri Design System

### Documentation

See **[https://kolibri-design-system.netlify.com](https://kolibri-design-system.netlify.com)** for the latest design system documentation.

This contains resources for designers and developers who are building Kolibri products using the design system patterns and the shared UI library.

### Shared UI library

The shared UI library is a node package containing front-end components, utilities, and style definitions supporting the Kolibri Design System and used in Learning Equality applications.

It is currently hosted only on Github, not NPM. To add a particular pinned version to your project using `yarn`, run:

```bash
yarn add https://github.com/learningequality/kolibri-design-system
```

You can also specify a particular version of the design system. For example, this would specify `v1.0.1`:

```bash
yarn add https://github.com/learningequality/kolibri-design-system#v1.0.1
```

 Importable components and utilities are available under the `lib` path, for example:

```javascript
import KButton from 'kolibri-design-system/lib/KButton';
```

Refer to the [documentation](https://kolibri-design-system.netlify.com) for specifics.

The shared UI library is made available as source code, not a built distribution, and requires the external application to supply both build and runtime dependencies such as webpack, Vue, and Sass. These requirements will eventually be specified in `/package.json` as `peerDependencies`.

The public API of the shared UI library is _only_ that which is documented in the documentation. Functionality which is not documented should be considered either experimental or a private implementation detail.

## Development

How to make updates to the Kolibri Design System

### Overview

The design system has two primary components, the shared UI library (under `/lib`) and the documentation site (under `/docs`).

The shared UI library code is built from Vue, Javascript, and Sass. It requires the dependencies specified under the `dependencies` and `peerDependencies` attributes of `/package.json`. An external application will import and functionality from `kolibri-design-system/lib` as [described in the documentation](https://kolibri-design-system.netlify.com), and then generate build artifacts as necessary for the application's specific deployment or distribution requirements.

The documentation site is built using [NuxtJS](https://nuxtjs.org/), [Storybook](https://storybook.js.org/), and [Vue](https://vuejs.org/). It requires the dependencies specified in the `devDependencies` attribute of `/package.json` to build and run. The documentation site can either be run locally using the Node.js development server, or exported as a static site and uploaded to a CDN.

### Running the documentation locally

Clone this repository using [Git](https://help.github.com/en/github/getting-started-with-github/set-up-git), optionally [forking](https://help.github.com/en/github/getting-started-with-github/fork-a-repo) it first if you plan to submit changes.

Install the dependencies using `yarn`:

```bash
cd design-system
yarn install
```

Run the development server using

```bash
yarn dev
```

This will launch a documentation development server at `http://localhost:3000/`. It also launches a linter and auto-formatter based on the [`kolibri-tools`](https://github.com/learningequality/kolibri/tree/develop/packages/kolibri-tools) package.

There are few other commands available:

```bash
yarn dev-only    # launch just the dev server without the linter and auto-formatter
yarn generate    # build a static version of the site for upload to CDN
yarn lint-fix    # run the linter and auto-formatter once
yarn lint-watch  # run the linter in watch mode, without the auto-formatter
```

### Making changes

The documentation site is built using [NuxtJS](https://nuxtjs.org/) (configured in `/nuxt.config.js`), [Storybook](https://storybook.js.org/), and [Vue](https://vuejs.org/).

The directories and files in `/docs/pages/` are mapped by NuxtJS to URLs. Start with those files for making updates to documentation content.

Common components used in the documentation site are in `/docs/common/`, and the primary page layout is `/docs/layouts/default.vue`.

The documentation site is intentionally built _without_ depending on the Kolibri shared UI library in order to make sure the UI library can be updated and modified independently of the design system documentation site. At some point we may want to revisit this decision, but in the meantime please limit references to `/lib` files to examples of usage.

### Deployment

The documentation site is currently deployed to Netlifty automatically when changes are pushed to one of the primary branches. This is configured in the `/netlify.toml` file and authenticated with the [Netlify GitHub app](https://docs.netlify.com/configure-builds/repo-permissions-linking/#authentication-with-the-netlify-github-app).

Longer-term, we will likely want to transition to Google Cloud for more control of the deployment. Specifically, we'll want to
