# SVITE with Tailwind Template

This template setups a project with Svelte, Svite and Tailwind

## Automated 

``` sh
npx bam github:hyper63/svite-template [PROJECT]
```

## Manual Steps

``` sh
mkdir myproject
cd myproject
yarn init -y
yarn add -D svelte svite postcss@7 tailwindcss@compat
touch postcss.config.js tailwind.config.js svelte.config.js
mkdir public src
touch public/favicon.svg
touch index.html
touch src/index.js src/index.css src/App.svelte
yarn add -D postcss-import@12
yarn add -D postcss-preset-env
yarn add -D svelte-preprocess
```

postcss.config.js

``` js
module.exports = {
  plugins: [require('postcss-import'), require('tailwindcss'), require('postcss-preset-env')({ stage: 1 })],
};
```

tailwind.config.js

``` js
const { tailwindExtractor } = require('tailwindcss/lib/lib/purgeUnusedStyles');

const svelteClassColonExtractor = (content) => {
  return content.match(/(?<=class:)([a-zA-Z0-9_-]+)/gm) || [];
};

module.exports = {
  purge: {
    enabled: process.env.NODE_ENV === 'production',
    content: ['./src/**/*.svelte', './src/**/*.html', './src/**/*.css', './index.html'],
    preserveHtmlElements: true,
    options: {
      safelist: [/svelte-/],
      defaultExtractor: (content) => {
        // WARNING: tailwindExtractor is internal tailwind api
        // if this breaks after a tailwind update, report to svite repo
        return [...tailwindExtractor(content), ...svelteClassColonExtractor(content)];
      },
      keyframes: true,
    },
  },
  theme: {
    extend: {
    },
  },
  variants: {},
  plugins: [],
};

```

svelte.config.js

``` js
const { postcss } = require('svelte-preprocess');
module.exports = {
  preprocess: [postcss()],
};
```

index.html

``` html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Hello World</title>
  </head>
  <body>
    <script type="module" src="/src/index.js"></script>
  </body>
</html>
```

src/index.js

``` js
import App from './App.svelte';
import './index.css';

const app = new App({
  target: document.body,
});

export default app;
```

src/index.css

``` css
@import 'tailwindcss/base.css';
@import 'tailwindcss/components.css';
@import 'tailwindcss/utilities.css';
```

src/App.svelte

``` svelte 
<h1>Hello World</h1>
```

public/favicon.svg

``` svg
<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M14.828 14.828a4 4 0 01-5.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
</svg>
```

package.json

``` json
{
  ...
  "scripts": {
    "build": "svite build",
    "dev": "svite"
  }
}

```

## Congrats! You have setup a svite app with tailwindcss

## Acknowlegements

* Svite - https://github.com/dominikg/svite
* Svelte - https://svelte.dev
* Tailwindcss - https://tailwindcss.com


