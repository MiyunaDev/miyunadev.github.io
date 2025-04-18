<script lang="ts" setup>


import { ref } from "vue"
import { FaAndroid } from "vue-icons-plus/fa"

import CardApp from "./components/CardApp.vue"

</script>

<template>
  <div class="w-full h-full">
    <div class="flex relative flex-col md:flex-row w-full h-full bg-[#090909] py-2">
      <div class="w-full h-3/5 md:w-5/12 md:h-full flex flex-col justify-center items-center">
        <div>
          <p class="text-xl font-bold">Miyuna Developer</p>
          <a>Multiplatform series list and tracking</a>
        </div>
      </div>
      <div class="w-full h-2/5 md:w-7/12 md:h-full relative flex flex-col justify-center-safe items-center">
        <div class="w-full h-full absolute top-0 left-0 bg-gradient-to-b md:bg-gradient-to-r from-[rgba(0,0,0,0.9)] to-[rgba(0,0,0,0.4)] ">
        </div>
        <img class="h-full md:h-4/6 object-cover" src="/src/assets/kiirohana_library.jpeg">
      </div>
    </div>

    <div class="bg-[#2c0026] p-4">
      <div class="p-5">
        <div class="p-4 shadow-2xl">
          <p class="text-center font-bold text-3xl my-2">What is Miyuna?</p>
          <p class="md:text-lg p-10">We are a multiplatform Read & Watch Series/Novel/Comic app development team. You can enjoy the app for multiple platforms and variety of sweets that you can choose from</p>
          

          <div class="bg-[#490959] p-2 rounded-2xl my-2">
            <p class="text-2xl font-semibold p-4 text-center">Do it all in one place</p>
              <div class="w-full flex flex-col md:flex-row justify-center items-center">
                <img class="w-full md:w-2/6 object-contain rounded-2xl" src="/src/assets/kiirohana_library.jpeg">
                <p class="w-full md:w-4/6 p-8 md:text-lg">We provide an app where you can read and watch the shows or series you follow in just one app and easy. Whether reading Manga, Light Novel or Web Novel then Watch your favorite Drama, Anime and Movies with your choice of language.</p>
              </div>
          </div>


        </div>
      </div>

      <div class="p-5">
        <div class="p-4 shadow-2xl">
          <p class="text-center font-bold text-3xl my-2">Download</p>
          <p class="md:text-lg p-10">We have many flavor for your choices with different device support. Please read deve support before installation</p>
          
          <CardApp appName="Kiirohana" preview="/src/assets/kiirohana_library.jpeg" :production="false" />
        </div>
      </div>
    </div>
  </div>
</template>