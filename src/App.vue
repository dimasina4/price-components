<template>
  <div id="app">
    <PriceComponents />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import PriceComponents from './views/PriceComponents.vue';

@Component({
  components: {
    PriceComponents,
  },
})
export default class App extends Vue {}
</script>
