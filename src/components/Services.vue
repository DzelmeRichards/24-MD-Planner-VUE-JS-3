<template>
  <div class="services__container">
    <div class="services__card">
      <h3 class="services__title">Services</h3>
      <div
        :class="{
          services__selected: service.isSelected,
          services__unselected: !service.isSelected,
          service,
        }"
        :key="index"
        v-for="(service, index) in services"
        @click="
          {
            toggleSelectedServices(service);
            sumTotalServices();
          }
        "
      >
        <span>{{ service.title }}</span>
        <span>${{ service.price.toFixed(2) }}</span>
      </div>
      <hr class="sevices__hr" />
      <div class="sevices__total-container">
        <span>Total:</span>
        <span>${{ sumTotalServices().toFixed(2) }}</span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export type Service = {
  title: string;
  price: number;
  isSelected: boolean;
};

import { defineComponent } from "vue";
export default defineComponent({
  name: "Services-component",
  data: () => ({
    services: [
      {
        title: "Web Development",
        price: 300,
        isSelected: false,
      },
      {
        title: "Design",
        price: 400,
        isSelected: false,
      },
      {
        title: "Integration",
        price: 250,
        isSelected: false,
      },
      {
        title: "Training",
        price: 220,
        isSelected: false,
      },
    ],
  }),
  methods: {
    toggleSelectedServices(service: Service) {
      service.isSelected = !service.isSelected;
    },
    sumTotalServices(): number {
      let total = 0;
      this.services.forEach((service) => {
        if (service.isSelected) {
          total += service.price;
        }
      });
      return total;
    },
  },
});
</script>

<style lang="scss">
@import "./services.scss";
</style>
