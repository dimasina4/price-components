<template>
  <div class="price-label">
    <input
      v-if="isEditMode"
      :value="label"
      @blur="saveChanges($event.target.value)"
      class="price-label__input"
      placeholder="Label"
    />
    <span v-else :title="label" class="price-label__label">
      {{ label }}
    </span>
    <button
      v-if="isToggleable && !isEditMode"
      @click="toggleEdit"
      class="price-label__edit-button icon-button"
    >
      <span class="icon"></span>
    </button>
  </div>
</template>

<script lang="ts">
import { Component, Prop, VModel, Vue, Emit } from 'vue-property-decorator';

@Component
export default class PriceLabel extends Vue {
  @VModel({ type: String }) label!: string;
  @Prop({ type: Boolean }) staticEditMode!: boolean;
  @Prop({ type: Boolean }) isToggleable!: boolean;

  isEditMode = false;

  @Emit()
  blur() {
    return;
  }

  saveChanges(newValue: string) {
    this.toggleEdit();

    if (!newValue.trim()) {
      return;
    }

    this.label = newValue.trim();
    Vue.nextTick(() => {
      this.blur();
    });
  }

  toggleEdit() {
    if (!this.isToggleable) {
      return;
    }

    this.isEditMode = !this.isEditMode;
  }

  mounted() {
    this.isEditMode = this.staticEditMode;
  }
}
</script>

<style lang="scss" scoped>
.price-label {
  display: flex;
  justify-content: space-between;

  &__input {
    width: 100%;
  }

  &__label {
    line-height: 22px;
    margin-right: 4px;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
  }

  &__edit-button::before {
    background: url('/src/assets/edit.svg') no-repeat;
  }
}
</style>
