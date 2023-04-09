<script setup lang="ts">
   import { defineProps } from 'vue';

   defineProps<{
      data: any;
   }>();

   const compactNumber = Intl.NumberFormat('en-US', {
      notation: 'compact'
   }).format;
   const formatNumber = Intl.NumberFormat('en-US').format;
</script>

<template>
   <div class="country-modal">
      <div class="content-container">
         <h1 class="title">
            <img :src="data.flags.png" alt="Country Flag" class="flag-image">
            <span>{{ data.name.common }} ({{ data.capital?.length ? data.capital[0] : 'X' }})</span>
         </h1>
         <div class="info-container">
            <div class="info">
               <strong>Official Name:</strong>
               {{ data.name.official }}
            </div>
            <div class="info">
               <strong>Top-Level Domain:</strong>
               {{ data.tld[0] }}
            </div>
            <div class="info">
               <strong>Currencies:</strong>
               {{ data.currencies ? Object.keys(data.currencies).map((key) => `${data.currencies[key].name} (${data.currencies[key].symbol})`).join(', ') : 'N/A' }}
            </div>
            <div class="info">
               <strong>Region:</strong>
               {{ data.region }}
            </div>
            <div class="info">
               <strong>Languages:</strong>
               {{ data.languages ? Object.values(data.languages).join(', ') : 'N/A' }}
            </div>
            <div class="info">
               <strong>Population:</strong>
               {{ compactNumber(data.population) }}
            </div>
            <div class="info">
               <strong>Area:</strong>
               {{ formatNumber(data.area) }} km²
            </div>
            <div class="info">
               <strong>Timezones:</strong>
               {{ data.timezones.join(', ') }}
            </div>
         </div>
      </div>
   </div>
</template>

<style scoped>
   .country-modal {
      position: fixed;
      
      background-color: rgba(0, 0, 0, .5);

      width: 100%;
      height: 100%;

      top: 0;

      display: flex;
      justify-content: center;
      align-items: center;
   }

   .content-container {
      background-color: var(--lightgray);

      width: 500px;

      padding: 15px;

      border-radius: 10px;

      animation: .2s ease fadeIn;
   }

   @keyframes fadeIn {
      from {
         transform: scale(0);
         opacity: 0;
      }
      to {
         opacity: 1;
      }
   }

   .title {
      font-size: 1.2rem;

      display: flex;
      align-items: center;
   }

   .flag-image {
      width: 2.25rem;
      
      margin-right: .5rem;
   }

   .info-container {
      padding: 10px;

      background-color: var(--darkgray);

      border-radius: 10px;

      margin-top: 10px;

      display: flex;
      flex-direction: column;
      gap: 5px;
   }
</style>