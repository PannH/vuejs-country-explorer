<script setup lang="ts">
   import { defineProps, ref } from 'vue';
   import CountryModal from './CountryModal.vue';

   defineProps<{
      data: any;
   }>();

   const showModal = ref(false);
</script>

<template>
   <div class="country-card" @click="showModal = true">
      <img :src="data.flags.png" alt="Country Flag" class="flag-image">
      <div class="info-container">
         <span class="name">{{ data.name.common }} ({{ data.capital?.length ? data.capital[0] : 'X' }})</span>
      </div>
   </div>
   <CountryModal :data="data" v-if="showModal" @click="showModal = false"/>
</template>

<style scoped>
   .country-card {
      background-color: var(--gray);

      border-radius: 10px;

      overflow: hidden;

      transition: .1s ease-in-out;

      box-shadow: 0px 5px 20px rgba(0, 0, 0, .1);

      max-width: 200px;
      height: fit-content;

      animation: .3s ease fadeIn;
   }

   @keyframes fadeIn {
      from {
         transform: translateY(10%);
         opacity: 0;
      }
      to {
         opacity: 1;
      }
   }

   .country-card:hover {
      cursor: pointer;
      transform: scale(1.05);
   }

   .flag-image {
      width: 100%;
   }

   .info-container {
      text-align: center;
      margin: 15px;
   }

   .info-container .name {
      font-size: 1.2rem;
   }
</style>