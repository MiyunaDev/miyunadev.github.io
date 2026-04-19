<script lang="ts" setup>
import { computed, ref } from 'vue'
import CardApp from './components/CardApp.vue'
import kiirohanaPreview from "./assets/kiirohana_library.jpeg"

import kiirohanaIcon from "./assets/icon/kiirohana.png"
import hanatsukiIcon from "./assets/icon/hanatsuki.png"
import sakihanaIcon from "./assets/icon/sakihana.png"
import kohibanaIcon from "./assets/icon/kohibana.png"
import shirohanaIcon from "./assets/icon/shirohana.png"

import {
  PiMagnifyingGlass,
  PiX,
  PiDownloadSimpleDuotone,
  PiAndroidLogo,
  PiWindowsLogo,
  PiLinuxLogo,
  PiGlobeDuotone,
  PiCpuDuotone,
  PiCodeDuotone,
  PiSparkleDuotone
} from 'vue-icons-plus/pi'

type AppItem = {
  name: string
  icon: string
  prod: boolean
  comingSoon: boolean
  personalPlatforms: { name: string; icon: any }[]
  technologies: string[]
  languages: string[]
}

const apps: AppItem[] = [
  {
    name: 'Shirohana',
    icon: shirohanaIcon,
    prod: false,
    comingSoon: true,
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ['Apache Cordova'],
    languages: ['TypeScript']
  },
  {
    name: 'Kiirohana',
    icon: kiirohanaIcon,
    prod: false,
    comingSoon: true,
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ['Apache Cordova'],
    languages: ['TypeScript']
  },
  {
    name: 'Hanatsuki',
    icon: hanatsukiIcon,
    prod: false,
    comingSoon: true,
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ['Apache Cordova'],
    languages: ['JavaScript']
  },
  {
    name: 'Sakihana',
    icon: sakihanaIcon,
    prod: false,
    comingSoon: true,
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ['Apache Cordova'],
    languages: ['JavaScript']
  },
  {
    name: 'Kohibana',
    icon: kohibanaIcon,
    prod: false,
    comingSoon: true,
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ['Apache Cordova'],
    languages: ['JavaScript']
  }
]

const search = ref('')
const selectedPlatform = ref('All')
const selectedTech = ref('All')
const selectedLang = ref('All')

const platformOptions = computed(() => {
  const list = apps.flatMap(i => i.personalPlatforms.map(p => p.name))
  return ['All', ...new Set(list)]
})

const techOptions = computed(() => {
  const list = apps.flatMap(i => i.technologies)
  return ['All', ...new Set(list)]
})

const langOptions = computed(() => {
  const list = apps.flatMap(i => i.languages)
  return ['All', ...new Set(list)]
})

const filteredApps = computed(() => {
  return apps.filter(app => {
    const byName =
      app.name.toLowerCase().includes(search.value.toLowerCase())

    const byPlatform =
      selectedPlatform.value === 'All' ||
      app.personalPlatforms.some(
        p => p.name === selectedPlatform.value
      )

    const byTech =
      selectedTech.value === 'All' ||
      app.technologies.includes(selectedTech.value)

    const byLang =
      selectedLang.value === 'All' ||
      app.languages.includes(selectedLang.value)

    return byName && byPlatform && byTech && byLang
  })
})

const resetFilter = () => {
  search.value = ''
  selectedPlatform.value = 'All'
  selectedTech.value = 'All'
  selectedLang.value = 'All'
}
</script>

<template>
  <div class="min-h-screen bg-slate-950 text-white">

    <!-- HERO -->
    <section class="relative overflow-hidden">
  <img :src="kiirohanaPreview" class="absolute inset-0 w-full h-full object-cover opacity-20" />

  <div class="relative max-w-7xl mx-auto px-6 py-28 text-center">
    <p class="text-fuchsia-300 font-semibold tracking-widest uppercase">
      Miyuna Ecosystem
    </p>

    <h1 class="text-6xl font-black mt-4">
      Read. Watch. Own Your Platform.
    </h1>

    <p class="max-w-3xl mx-auto mt-6 text-lg text-slate-300">
      Miyuna is a multiplatform private media ecosystem powered by Honoka server.
      Read novels, comics, PDFs, web content, and stream your media
      across Android, Windows, Linux, and Web.
    </p>

    <div class="mt-8 flex gap-4 justify-center flex-wrap">
      <a class="px-6 py-3 rounded-xl bg-fuchsia-600 hover:bg-fuchsia-500">
        Explore Apps
      </a>

      <a class="px-6 py-3 rounded-xl border border-white/20">
        Learn Ecosystem
      </a>
    </div>
  </div>
</section>

<section class="max-w-7xl mx-auto px-6 py-20">
  <h2 class="text-4xl font-bold text-center mb-12">
    Beautiful Across Devices
  </h2>

  <div class="grid md:grid-cols-3 gap-6">
    <img :src="kiirohanaPreview" class="rounded-3xl shadow-xl" />
    <img :src="kiirohanaPreview" class="rounded-3xl shadow-xl" />
    <img :src="kiirohanaPreview" class="rounded-3xl shadow-xl" />
  </div>
</section>

<section class="max-w-7xl mx-auto px-6 py-20">
  <h2 class="text-4xl font-bold text-center mb-12">
    How It Works
  </h2>

  <div class="grid md:grid-cols-3 gap-6">
    <div class="card">1. Install Honoka Server</div>
    <div class="card">2. Connect Miyuna App</div>
    <div class="card">3. Enjoy Private Media Anywhere</div>
  </div>
</section>

    <!-- CONTENT -->
    <main class="max-w-7xl mx-auto px-6 py-10 space-y-8">

      <!-- FILTER PANEL -->
      <section
        class="rounded-3xl border border-white/10 bg-white/5 backdrop-blur-xl p-6 shadow-2xl"
      >
        <div class="flex items-center gap-2 mb-5">
          <PiMagnifyingGlass class="w-5 h-5 text-fuchsia-300" />
          <h2 class="text-xl font-bold">Search & Filters</h2>
        </div>

        <div class="grid md:grid-cols-4 gap-4">
          <input
            v-model="search"
            placeholder="Search app..."
            class="px-4 py-3 rounded-xl bg-slate-900 border border-white/10 outline-none focus:border-fuchsia-400"
          />

          <select
            v-model="selectedPlatform"
            class="px-4 py-3 rounded-xl bg-slate-900 border border-white/10"
          >
            <option
              v-for="item in platformOptions"
              :key="item"
              :value="item"
            >
              {{ item }}
            </option>
          </select>

          <select
            v-model="selectedTech"
            class="px-4 py-3 rounded-xl bg-slate-900 border border-white/10"
          >
            <option
              v-for="item in techOptions"
              :key="item"
              :value="item"
            >
              {{ item }}
            </option>
          </select>

          <select
            v-model="selectedLang"
            class="px-4 py-3 rounded-xl bg-slate-900 border border-white/10"
          >
            <option
              v-for="item in langOptions"
              :key="item"
              :value="item"
            >
              {{ item }}
            </option>
          </select>
        </div>

        <div class="flex justify-between items-center mt-5">
          <p class="text-sm text-slate-400">
            Showing {{ filteredApps.length }} app(s)
          </p>

          <button
            @click="resetFilter"
            class="inline-flex items-center gap-2 px-4 py-2 rounded-xl bg-fuchsia-600 hover:bg-fuchsia-500 transition"
          >
            <PiX class="w-4 h-4" />
            Reset
          </button>
        </div>
      </section>

      <!-- APP LIST -->
      <section
        class="rounded-3xl border border-white/10 bg-white/5 backdrop-blur-xl p-6"
      >
        <div class="flex items-center gap-2 mb-6">
          <PiDownloadSimpleDuotone class="w-5 h-5 text-fuchsia-300" />
          <h2 class="text-2xl font-bold">Applications</h2>
        </div>

        <div class="grid md:grid-cols-2 xl:grid-cols-3 gap-6">
          <div
            v-for="app in filteredApps"
            :key="app.name"
            class="rounded-2xl border border-white/10 bg-slate-900/70 p-4 space-y-4"
          >
            <CardApp
              :appName="app.name"
              :appIcon="app.icon"
              :previews="[kiirohanaPreview]"
              :production="app.prod"
              :personalPlatforms="app.personalPlatforms"
              :comingSoon="app.comingSoon"
            />

            <!-- TECH -->
            <div>
              <p class="text-sm text-slate-400 mb-2 flex items-center gap-2">
                <PiCpuDuotone class="w-4 h-4" />
                Technology
              </p>

              <div class="flex flex-wrap gap-2">
                <span
                  v-for="tech in app.technologies"
                  :key="tech"
                  class="px-3 py-1 rounded-full bg-fuchsia-500/20 text-fuchsia-200 text-xs"
                >
                  {{ tech }}
                </span>
              </div>
            </div>

            <!-- LANGUAGE -->
            <div>
              <p class="text-sm text-slate-400 mb-2 flex items-center gap-2">
                <PiCodeDuotone class="w-4 h-4" />
                Language
              </p>

              <div class="flex flex-wrap gap-2">
                <span
                  v-for="lang in app.languages"
                  :key="lang"
                  class="px-3 py-1 rounded-full bg-cyan-500/20 text-cyan-200 text-xs"
                >
                  {{ lang }}
                </span>
              </div>
            </div>
          </div>
        </div>

        <div
          v-if="filteredApps.length === 0"
          class="text-center py-12 text-slate-400"
        >
          No applications found.
        </div>
      </section>
    </main>

    <!-- FOOTER -->
    <footer class="border-t border-white/10 mt-10">
      <div
        class="max-w-7xl mx-auto px-6 py-8 text-sm text-slate-400 flex flex-col md:flex-row justify-between gap-4"
      >
        <p>© {{ new Date().getFullYear() }} Miyuna Developer</p>
        <p>Modern UI • Responsive • Fast Filtering</p>
      </div>
    </footer>
  </div>
</template>