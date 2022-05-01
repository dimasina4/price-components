<template>
  <div class="price-field">
    <PriceLabel
      v-model="priceLabel"
      :static-edit-mode="staticLabelEditMode"
      :is-toggleable="isLabelToggleable"
      class="price-field__label"
      @blur="blur"
    />

    <PriceValue v-model="priceValue" class="price-field__value" @blur="blur" />

    <button
      v-if="isDeletable"
      @click="deleteField"
      class="price-field__delete-button icon-button"
    ></button>
  </div>
</template>

<script lang="ts">
import { Component, Vue, PropSync, Emit, Prop } from 'vue-property-decorator';
import PriceLabel from './PriceLabel.vue';
import PriceValue from './PriceValue.vue';

@Component({
  components: {
    PriceLabel,
    PriceValue,
  },
})
export default class PriceField extends Vue {
  @Prop({ type: Boolean }) staticLabelEditMode!: boolean;
  @Prop({ type: Boolean }) isLabelToggleable!: boolean;
  @Prop({ type: Boolean }) isDeletable!: boolean;

  @PropSync('label', { type: String }) priceLabel!: string;
  @PropSync('price', { type: Number }) priceValue!: number;

  @Emit()
  deleteField() {
    return;
  }

  @Emit()
  blur() {
    return;
  }
}
</script>

<style lang="scss" scoped>
.price-field {
  display: flex;

  > * {
    margin-right: 4px;

    &:last-child {
      margin-right: 0;
    }
  }

  &__label {
    width: 35%;
  }

  &__value {
    width: calc(65% - 38px);
  }

  &:hover,
  &:focus-within {
    ::v-deep .price-label__edit-button,
    .price-field__delete-button {
      visibility: visible;
    }
  }

  ::v-deep .price-label__edit-button,
  &__delete-button {
    visibility: hidden;
  }

  &__delete-button::before {
    background: url('/src/assets/garbage.svg') no-repeat;
  }
}
</style>
