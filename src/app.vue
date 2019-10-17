<script>
import appConfig from '@src/app.config'
import './design/index.scss'
// import './router/views/ion.rangeSlider.min'
import BootstrapVue from 'bootstrap-vue'

export default {
  BootstrapVue,
  page: {
    // All subcomponent titles will be injected into this template.
    titleTemplate(title) {
      title = typeof title === 'function' ? title(this.$store) : title
      return title ? `${title} | ${appConfig.title}` : appConfig.title
    },
    meta: [
        { name : 'Access-Control-Allow-Origin', content : 'http://localhost:8080'},
        { name : 'Access-Control-Allow-Credentials', content : true},
        { name : 'Access-Control-Allow-Methods', content : 'GET,PUT,POST,DELETE'},
        {'Access-Control-Allow-Origin' : 'http://localhost:8080'},
        {'Access-Control-Allow-Credentials' : true},
        {'Access-Control-Allow-Methods' : 'GET,PUT,POST,DELETE'}
    ],
    script: [
      {src : "https://maxcdn.bootstrapcdn.com/bootstrap/3.4.0/js/bootstrap.min.js"}
    ]
    // link: [
    //     {
    //         'href': './design/index.scss',
    //         'rel' : 'stylesheet',
    //         'type':'text/css'
    //     }
    // ],
  },
}
</script>

<template>
  <div id="app">
    <!--
    Even when routes use the same component, treat them
    as distinct and create the component again.
    -->
    <RouterView :key="$route.fullPath" />
  </div>
</template>

<!-- This should generally be the only global CSS in the app. -->
<style lang="scss">
// Allow element/type selectors, because this is global CSS.
// stylelint-disable selector-max-type, selector-class-pattern

// Design variables and utilities from src/design.
@import '@design/';
</style>
