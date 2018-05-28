# Prismic Vue.js Starter

> [Vue.js](https://vuejs.org) starter project in order to have content managed on [Prismic](https://prismic.io)

This project is based on [vuejs-templates/webpack](https://github.com/vuejs-templates/webpack)

## How to launch this project in your local environment

Run the following commands:

``` bash
npm install
npm run dev
```

Then you can access it at [http://localhost:8080](http://localhost:8080).
You'll see a tutorial that explain how to create your first Vue component filled with content retrieved from Prismic.

## How to remove the tutorial from this project

- Remove Tutorial import and route in src/router/index.js
- Delete Tutorial component file (rm src/components/Tutorial.vue)
- Delete Tutorial assets (rm -r src/assets/css/tutorial/ && rm -r src/assets/img/tutorial/)
- Remove vue-highlightjs dependency (npm uninstall vue-highlightjs)

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
