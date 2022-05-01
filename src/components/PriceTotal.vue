<template>
  <div class="price-total">Total: {{ total }} EUR/KG</div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';

@Component
export default class PriceTotal extends Vue {
  @Prop({ type: Number, default: 0 }) total!: number;
}
</script>

<style scoped></style>
