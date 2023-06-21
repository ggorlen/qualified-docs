# Qualified Docs

[![Netlify Status](https://api.netlify.com/api/v1/badges/27a6c94b-2bc0-4376-9a39-48ad05b5085f/deploy-status)](https://app.netlify.com/sites/fervent-mayer-2fb2ca/deploys)

Provides public & private documentation for Qualified.

[docs.qualified.io](https://docs.qualified.io)

## Documentation

### Quickstart:

All project dependencies can be installed automatically by running

```bash
yarn install
```

This uses Gridsome to develop the docs. You'll need to install gridsome globally, as in

```bash
npm install -g @gridsome/cli@0.3.4
```

Then inside the repository, run this command

```bash
gridsome develop
```

### Detailed docs on this theme

Additional documentation can be found [here](https://docc-theme.netlify.com/).

[Prism.js syntax highlighting](https://www.npmjs.com/package/prism-themes) can be adjusted in [MarkdownPage.vue](/src/templates/MarkdownPage.vue).


### Content Structure

The `/content` folder contains all markdown articles used within these docs. Each directory represents a level of depth. Up to 3 levels of depth are shown within the applications sidebar. The 2nd level of depth is treated only as a section header, so the only information the `/content/[depth 1 folder]/[depth 2 folder]/index.html` file should contain is `title` and `order` front-matter. 

### API Docs

The API docs are generated based on JSON files exported from the main Qualified Rails API. There are two files, one that only contains public APIs, and one that contains both public and private APIs.

The JSON files live under `/content/api`.

The private API is available at [docs.qualified.io/integrations/custom-integrations/private-api/](https://docs.qualified.io/integrations/custom-integrations/private-api/)

### Remark Markdown Extensions

#### Content Containers
![Content Containers](https://p191.p3.n0.cdn.getcloudapp.com/items/9ZuB6pXv/Image%202020-11-04%20at%2012.45.31%20PM.png?source=viewer&v=19cacb18db0da80c607a80a983100112)

