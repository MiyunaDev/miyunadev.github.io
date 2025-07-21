<script lang="ts" setup>
import { ref } from 'vue'
import CardApp from './components/CardApp.vue'

import {
  PiPlayDuotone,
  PiFlowerDuotone,
  PiBrainDuotone,
  PiLockDuotone,
  PiPuzzlePieceDuotone,
  PiLightbulbDuotone,
  PiGearSixDuotone,
  PiWifiSlashDuotone,
  PiDeviceMobileDuotone,
  PiPaintBrushDuotone,
  PiBookOpenDuotone,
  PiGlobeDuotone,
  PiBookmarkDuotone,
  PiSlidersHorizontalDuotone,
  PiWarningCircleDuotone,
  PiLockKeyDuotone,
  PiHandDuotone,
  PiShieldCheckDuotone,
  PiDownloadSimpleDuotone,
  PiProhibitInset,
  PiWarningDiamondDuotone,
  PiAndroidLogo,
  PiWindowsLogo,
  PiLinuxLogo
} from 'vue-icons-plus/pi'

let apps = [
  {
    name: 'Kiirohana', prod: false, personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ]
  },
  {
    name: 'Hanatsuki', prod: false, personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ], comingSoon: true
  },
  {
    name: 'Sakihana', prod: false, personalPlatforms: [], comingSoon: true
  },
  {
    name: 'Kohibana', prod: false, personalPlatforms: [], comingSoon: true
  },
]
</script>

<template>
  <div class="w-full h-full text-white font-sans">
    <!-- HERO SECTION -->
    <div class="flex flex-col md:flex-row w-full h-full bg-gradient-to-br from-black via-gray-900 to-gray-800 py-6">
      <div class="w-full md:w-5/12 flex flex-col justify-center items-center px-6 text-center space-y-2">
        <p class="text-3xl font-bold text-pink-500 drop-shadow">Miyuna Developer</p>
        <p class="text-gray-300">Multiplatform series list and tracking</p>
      </div>
      <div class="w-full md:w-7/12 relative">
        <div class="absolute inset-0 bg-gradient-to-r from-black via-transparent to-black opacity-70"></div>
        <img src="/src/assets/kiirohana_library.jpeg"
          class="object-cover w-full h-full rounded-xl opacity-30 blur-sm" />
      </div>
    </div>

    <!-- MAIN CONTENT -->
    <div class="bg-[#2c0026] px-4 py-10 md:px-10 space-y-12">

      <!-- WHAT IS MIYUNA -->
      <section class="text-center">
        <h2 class="text-4xl font-bold text-purple-300 mb-4 drop-shadow">
          What is <span class="text-pink-500">Miyuna</span>?
        </h2>
        <p class="text-lg text-gray-200 max-w-3xl mx-auto leading-relaxed">
          <strong>Miyuna</strong> is a multiplatform <em>Read & Watch</em> experience — novels, comics, and series,
          all in one beautifully crafted app.
        </p>
      </section>

      <!-- FEATURE HIGHLIGHT -->
      <section class="bg-gradient-to-br from-pink-600 via-purple-700 to-indigo-800 p-6 rounded-3xl shadow-xl">
        <h3 class="text-3xl font-semibold text-center mb-4 flex items-center justify-center gap-2">
          <PiPlayDuotone class="w-6 h-6" />
          Do it All in One Place
        </h3>
        <p class="md:text-lg text-center md:w-3/4 mx-auto leading-relaxed text-white/90">
          With Miyuna, you can read and watch the content you love — all in one app.
        </p>
      </section>

      <!-- WELCOME -->
      <section
        class="bg-gradient-to-br from-fuchsia-800 via-pink-600 to-rose-700 p-6 rounded-3xl text-center shadow-md">
        <h3 class="text-4xl font-bold mb-2 flex justify-center items-center gap-2">
          <PiFlowerDuotone class="w-6 h-6 text-pink-100" />
          Welcome to Miyuna
        </h3>
        <p class="text-lg md:px-12 text-white/90">
          A world where stories come to life. <br /><br />
          <strong class="text-pink-200">Miyuna</strong> isn't just another app — it's your personal media sanctuary.
        </p>
      </section>

      <!-- CLIENT -->
      <section class="bg-[#431042] p-6 rounded-3xl shadow-md">
        <h3 class="text-3xl font-semibold text-center text-pink-200 mb-4 flex justify-center gap-2 items-center">
          <PiBrainDuotone class="w-6 h-6" />
          Honoka – Your Private Brain
        </h3>
        <ul class="space-y-3 px-6 text-pink-100 text-lg">
          <li class="flex items-start gap-3">
            <PiLockDuotone class="w-6 h-6" /> <strong>Self-hosted</strong>
          </li>
          <li class="flex items-start gap-3">
            <PiPuzzlePieceDuotone class="w-6 h-6" /> <strong>Integrates well</strong>
          </li>
          <li class="flex items-start gap-3">
            <PiLightbulbDuotone class="w-6 h-6" /> <strong>Lightweight</strong>
          </li>
          <li class="flex items-start gap-3">
            <PiGearSixDuotone class="w-6 h-6" /> <strong>Configurable</strong>
          </li>
          <li class="flex items-start gap-3">
            <PiWifiSlashDuotone class="w-6 h-6" /> <strong>Private by design</strong>
          </li>
        </ul>
      </section>

      <!-- APP -->
      <section class="bg-[#5a1752] p-6 rounded-3xl shadow-md">
        <h3 class="text-3xl font-semibold text-center text-pink-200 mb-4 flex justify-center gap-2 items-center">
          <PiDeviceMobileDuotone class="w-6 h-6" />
          Miyuna App – One App to Rule It All
        </h3>
        <ul class="space-y-3 px-6 text-pink-100 text-lg">
          <li class="flex items-start gap-3">
            <PiPaintBrushDuotone class="w-6 h-6" /> <strong>Beautiful design</strong>
          </li>
          <li class="flex items-start gap-3">
            <PiBookOpenDuotone class="w-6 h-6" /> <strong>Unified reading</strong>
          </li>
          <li class="flex items-start gap-3">
            <PiGlobeDuotone class="w-6 h-6" /> <strong>Multiplatform</strong>
          </li>
          <li class="flex items-start gap-3">
            <PiBookmarkDuotone class="w-6 h-6" /> <strong>Progress tracking</strong>
          </li>
          <li class="flex items-start gap-3">
            <PiSlidersHorizontalDuotone class="w-6 h-6" /> <strong>Customizable</strong>
          </li>
        </ul>
      </section>

      <!-- LICENSE -->
      <section class="bg-[#6a002e] p-6 rounded-3xl shadow-md">
        <h3 class="text-3xl font-bold text-center text-yellow-300 mb-4 flex justify-center gap-2 items-center">
          <PiWarningCircleDuotone class="w-6 h-6" />
          Use Responsibly, Use Privately
        </h3>
        <ul class="space-y-4 px-4 text-red-200 text-lg">
          <li class="flex items-start gap-3">
            <PiProhibitInset class="w-6 h-6" /> <strong>No redistribution</strong>
          </li>
          <li class="flex items-start gap-3">
            <PiLockKeyDuotone class="w-6 h-6" /> <strong>You manage your data</strong>
          </li>
          <li class="flex items-start gap-3">
            <PiWarningDiamondDuotone class="w-6 h-6" /> <strong>Misuse voids license</strong>
          </li>
          <li class="flex items-start gap-3">
            <PiHandDuotone class="w-6 h-6" /> <strong>Private use only</strong>
          </li>
          <li class="flex items-start gap-3">
            <PiShieldCheckDuotone class="w-6 h-6 text-yellow-300" /> <strong>Respect rights</strong>
          </li>
        </ul>
      </section>

      <!-- DOWNLOAD -->
      <!-- DOWNLOAD SECTION -->
      <section class="bg-[#330024] p-6 rounded-3xl shadow-xl space-y-10">
        <h3 class="text-3xl font-bold text-center mb-2 text-purple-200 flex justify-center gap-2 items-center">
          <PiDownloadSimpleDuotone class="w-6 h-6" />
          Download
        </h3>

        <!-- Tambahkan Informasi Honoka Dulu -->
        <!-- 👇 Bagian ini dulu -->
        <section class="bg-[#4a143e] border border-pink-600 p-6 rounded-2xl shadow-md text-center text-white space-y-4">
          <h3 class="text-2xl font-bold text-pink-300 flex justify-center items-center gap-2">
            <PiLockDuotone class="w-6 h-6" />
            Honoka Required
          </h3>
          <p class="text-white/90 max-w-2xl mx-auto leading-relaxed">
            All client apps require the <strong>Honoka</strong> backend to function properly.
            You must set up your own self-hosted server first.
            Full installation instructions and system releases are available on the official repository.
          </p>
          <a href="https://github.com/MiyunaDev/Honoka" target="_blank"
            class="inline-flex items-center gap-2 px-5 py-3 rounded-lg bg-gradient-to-r from-pink-700 via-purple-700 to-indigo-700 text-white font-semibold shadow-md hover:brightness-110 transition">
            <PiDownloadDuotone class="w-5 h-5" />
            Go to Honoka Repository
          </a>
        </section>

        <!-- Baru Flavor CardApp -->
        <p class="text-lg text-center text-white/80">
          We have many flavors for your device. Please read compatibility details before installing.
        </p>

        <div class="grid md:grid-cols-2 gap-6">
          <CardApp v-for="app in apps" :key="app.name" :appName="app.name"
            :previews="['/src/assets/kiirohana_library.jpeg']" :production="app.prod"
            :personalPlatforms="app.personalPlatforms" :comingSoon="app?.comingSoon ?? false" />
        </div>
      </section>

      <!-- FOOTER -->
      <footer class="bg-black text-white/70 py-8 text-sm mt-20 border-t border-white/10">
        <div class="max-w-4xl mx-auto px-4 flex flex-col items-center space-y-3 text-center">
          <p class="text-white/80">
            Built with ♥ by <strong class="text-pink-400">Miyuna Developer</strong>
          </p>

          <p>
            This project is open-source and for <strong>private use only</strong>.
            Please respect the terms and creators.
          </p>

          <div class="flex gap-4 text-white/70">
            <a href="https://github.com/MiyunaDev" target="_blank" class="hover:text-pink-400 transition">
              GitHub
            </a>
            <a href="https://github.com/MiyunaDev/Honoka/blob/main/LICENSE.md" target="_blank"
              class="hover:text-pink-400 transition">
              License
            </a>
            <a href="https://github.com/MiyunaDev/Honoka/blob/main/DISCLAIMER.md" class="hover:text-pink-400 transition">
              Terms
            </a>
          </div>

          <p class="text-xs text-white/30 mt-2">
            © {{ new Date().getFullYear() }} Miyuna. All rights reserved.
          </p>
        </div>
      </footer>


    </div>
  </div>
</template>