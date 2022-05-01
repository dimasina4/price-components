<template>
  <fieldset class="price-components">
    <legend class="price-components__legend">Price Components</legend>

    <PriceTotal :total="totalPrice" class="price-components__total" />

    <PriceField
      :label.sync="basePrice.label"
      :price.sync="basePrice.price"
      class="price-components__field"
    />

    <PriceField
      v-for="componentPrice in componentPrices"
      :key="componentPrice.id"
      :label.sync="componentPrice.label"
      :price.sync="componentPrice.price"
      :is-label-toggleable="true"
      :is-deletable="true"
      @delete-field="deleteComponent(componentPrice.id)"
      class="price-components__field"
    />

    <PriceField
      :label.sync="newComponent.label"
      :price.sync="newComponent.price"
      :static-label-edit-mode="true"
      @blur="addComponent"
      class="price-components__field price-components__field--new-component"
    />
  </fieldset>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import PriceField from '../components/PriceField.vue';
import PriceTotal from '../components/PriceTotal.vue';
import uniqueId from '../common/utils/uniqueId';
import PriceValue from '../components/PriceValue.vue';

interface ComponentPrice {
  id: number;
  label: string;
  price: number;
}

@Component({
  components: {
    PriceField,
    PriceTotal,
    PriceValue,
  },
})
export default class PriceComponents extends Vue {
  componentPrices: ComponentPrice[] = [];
  basePrice: ComponentPrice = {
    id: 0,
    label: 'Base price',
    price: 1,
  };
  newComponent: ComponentPrice = {
    id: uniqueId(),
    label: '',
    price: 0,
  };

  get totalPrice() {
    const componentsTotalPrice = this.componentPrices.reduce(
      (total, componentPrice) => (total += componentPrice.price),
      0
    );

    return componentsTotalPrice + this.basePrice.price;
  }

  deleteComponent(id: number) {
    this.componentPrices = this.componentPrices.filter(
      (componentPrice) => componentPrice.id !== id
    );
  }

  addComponent() {
    if (!this.newComponent.label || !this.newComponent.price) {
      return;
    }

    this.componentPrices.push(this.newComponent);
    this.newComponent = {
      id: uniqueId(),
      label: '',
      price: 0,
    };
  }
}
</script>

<style lang="scss" scoped>
.price-components {
  max-width: 500px;

  &__total {
    text-align: right;
    padding-right: 26px;
  }

  &__field {
    margin-bottom: 8px;

    &--new-component {
      ::v-deep input {
        border-color: #ccc;
      }
    }

    &:last-child {
      margin-bottom: 0;
    }
  }
}
</style>
