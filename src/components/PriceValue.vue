<template>
  <input
    :value="price"
    @input="input($event.target.value)"
    type="number"
    min="0"
    placeholder="value"
    class="price-value"
    @keydown="checkInputKey"
    @blur="blur"
  />
</template>

<script lang="ts">
import { Component, Vue, VModel, Emit } from 'vue-property-decorator';

const RESTRICTED_KEYS = ['-', 'e'];

@Component
export default class PriceValue extends Vue {
  @VModel({ type: Number }) price!: number;
  @Emit()
  blur() {
    return;
  }

  @Emit()
  input(value: string) {
    return parseFloat(value) || 0;
  }

  checkInputKey(event: KeyboardEvent) {
    if (RESTRICTED_KEYS.includes(event.key)) {
      event.preventDefault();
    }
  }
}
</script>
