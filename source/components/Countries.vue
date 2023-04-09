<script setup lang="ts">
   import { ref, watchEffect } from 'vue';
   import axios from 'axios';
   import CountryCard from './CountryCard.vue';

   let allCountries: any[] = [];

   const countries = ref([] as any[]);
   const searchQuery = ref('');

   watchEffect(async () => {
      const API_URL = 'https://restcountries.com/v3.1/all';
      allCountries = (await axios.get(API_URL)).data;
      countries.value = allCountries;
   });

   function filterCountries() {

      if (searchQuery.value.length >= 1)
         countries.value = allCountries.filter((country) => {
            const query = searchQuery.value.toLowerCase();

            return country.name.common.toLowerCase().includes(query) ||
            country.name.official.toLowerCase().includes(query) ||
            (country.capital?.length ? country.capital[0].toLowerCase().includes(query) : false) ||
            country.region.toLowerCase().includes(query);
         });
      else 
         countries.value = allCountries;

   }
</script>

<template>
   <div class="search-bar-container">
      <div class="search-bar">
         <input type="text" v-model="searchQuery" placeholder='e.g.: "France", "Brussels", "Asia", ...' @input="filterCountries">
         <i class="fa-solid fa-magnifying-glass"></i>
      </div>
      <span>
         Powered by <a href="https://restcountries.com/" target="_blank">REST Countries</a>
      </span>
   </div>
   <section class="countries">
      <div class="container">
         <CountryCard :data="countryData" v-for="countryData in countries"/>
      </div>
   </section>
</template>

<style scoped>
   .search-bar-container {
      display: flex;
      flex-direction: column;
      align-items: center;
   }

   .search-bar-container span {
      margin-top: 10px;
   }

   .search-bar-container a {
      color: var(--blue);

      text-decoration: none;
   }

   .search-bar {
      margin-top: 90px;

      background-color: var(--gray);

      overflow: hidden;

      width: 500px;
      height: 45px;

      border: 2px solid var(--blue);
      border-radius: 10px;

      display: flex;
      justify-content: space-evenly;
      align-items: center;
   }

   .search-bar input {
      width: 100%;
      height: 100%;

      border: none;
      background-color: transparent;

      font-family: Poppins, sans-serif;
      font-size: 1rem;

      margin: 10px;
   }

   .search-bar input:focus {
      outline: none;
   }

   .search-bar i {
      font-size: 1.5rem;

      height: 100%;
      width: 60px;

      color: var(--blue);

      border-left: 2px solid var(--blue);

      display: flex;
      justify-content: center;
      align-items: center;
   }

   @media (width <= 560px) {
      .search-bar {
         width: 90%;
      }
   }

   .countries {
      display: flex;
      flex-direction: column;
      align-items: center;
   }

   .container {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      gap: 50px;

      margin: 100px 10%;
   }
</style>