# Laravel Form Builder

A lightweight Laravel package that helps manage server-side form building and integrates with a frontend form renderer.

This package provides server-side helpers, published assets, and a Laravel service provider. For frontend components and form rendering, use the companion npm package:

- npm: https://www.npmjs.com/package/@dcodegroup-au/form-builder
- package name: @dcodegroup-au/form-builder

Features

- Laravel service provider to integrate form-builder features into your app
- Publishable frontend assets (JS/CSS) to integrate with your application frontend
- Designed to work with the @dcodegroup-au/form-builder npm package for rendering and interactivity

Installation (PHP)

Require via Composer:

```bash
composer require dcodegroup/laravel-form-builder
```

The package registers the service provider automatically via Composer's extra.laravel.providers. This package does not publish frontend assets. For frontend integration, install and import the companion npm package @dcodegroup-au/form-builder into your frontend build, or copy the compiled assets from node_modules into your public assets as part of your build pipeline.

Frontend integration (npm)

Install the frontend package into your JS project:

```bash
# npm
npm install @dcodegroup-au/form-builder --save

# or yarn
yarn add @dcodegroup-au/form-builder
```

Then import the library and styles in your entry file (Vite, Webpack, Mix, etc.):

```js
import '@dcodegroup-au/form-builder/dist/form-builder.css';
import FormBuilder from '@dcodegroup-au/form-builder';

// initialize or mount components per the npm package docs
```

Alternatively, include compiled assets produced by your frontend build in Blade templates. Choose the approach that matches your stack:

- Vite (Laravel + Vite): import the package in your resources/js entry and let Vite handle bundling and HMR.

```js
// resources/js/app.js
import '@dcodegroup-au/form-builder/dist/form-builder.css';
import FormBuilder from '@dcodegroup-au/form-builder';
```

Then load the compiled entry in Blade:

```blade
@vite(['resources/js/app.js'])
```

- Laravel Mix / Webpack: copy or require the dist files from node_modules in webpack.mix.js:

```js
mix.js('resources/js/app.js', 'public/js')
   .copy('node_modules/@dcodegroup-au/form-builder/dist/form-builder.js', 'public/js/vendor/form-builder.js')
   .copy('node_modules/@dcodegroup-au/form-builder/dist/form-builder.css', 'public/css/vendor/form-builder.css');
```

Then include with mix():

```blade
<link rel="stylesheet" href="{{ mix('css/vendor/form-builder.css') }}">
<script src="{{ mix('js/vendor/form-builder.js') }}" defer></script>
```

Adjust paths to match your build output and deployment conventions.

Usage



- Use the Laravel helpers and service provider for preparing form data on the server side.
- Use the frontend npm package to render forms, handle client-side validation, and submit data asynchronously.

Refer to the npm package documentation for frontend component usage: https://www.npmjs.com/package/@dcodegroup-au/form-builder

Testing

Run package tests (if provided):

```bash
composer test
```

Contributing

Contributions are welcome. Please open issues or pull requests and follow repository conventions.

License

MIT — see LICENSE.md for details.
