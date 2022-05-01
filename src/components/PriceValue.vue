<template>
  <input
    :value="formattedPrice"
    @input="input($event.target.value)"
    type="number"
    min="0"
    placeholder="value"
    class="price-value"
    :title="price"
    @keydown="checkInputKey"
    @focus="isFocused = true"
    @blur="blur"
  />
</template>

<script lang="ts">
import { Component, Vue, VModel, Emit } from 'vue-property-decorator';

const RESTRICTED_KEYS = ['-', 'e'];

@Component
export default class PriceValue extends Vue {
  @VModel({ type: Number }) price!: number;

  isFocused = false;

  @Emit()
  blur() {
    this.isFocused = false;
    return;
  }

  @Emit()
  input(value: string) {
    return parseFloat(value) || 0;
  }

  get formattedPrice() {
    return this.isFocused
      ? this.price
      : this.price.toFixed(this.decimalDigitsCount);
  }

  get decimalDigitsCount() {
    return this.price.toString().split('.')[1]?.length > 2 ? 2 : 1;
  }

  checkInputKey(event: KeyboardEvent) {
    if (RESTRICTED_KEYS.includes(event.key)) {
      event.preventDefault();
    }
  }
}
</script>
