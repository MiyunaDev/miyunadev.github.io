Aku ingin kamu membantuku mengisi seusai itu. Tanpa merubah tampilan sama sekali, dan tetap menjaga konsistensi nya.
Ubah bagian App.vue ini cocok dengan itu

<script lang="ts" setup>
import { computed, ref } from 'vue'
import CardApp from './components/CardApp.vue'
import kiirohanaPreview from "./assets/kiirohana_library.jpeg"

import kiirohanaIcon from "./assets/icon/kiirohana.png"
import hanatsukiIcon from "./assets/icon/hanatsuki.png"
import sakihanaIcon from "./assets/icon/sakihana.png"
import kohibanaIcon from "./assets/icon/kohibana.png"
import shirohanaIcon from "./assets/icon/shirohana.png"
import honokaIcon from "./assets/icon/honoka.png"
import shinobuIcon from "./assets/icon/shinobu.png"
import upcomingIcon from "./assets/icon/upcoming.png"

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
  PiSparkleDuotone,
  PiMonitorDuotone,
  PiDeviceMobileDuotone,
  PiPlayDuotone,
  PiBookOpenDuotone,
  PiQuestionMark,
  PiCircleNotch,
  PiMicrosoftOutlookLogo,
  PiDevices,
  PiDotsNine,
  PiAmazonLogo,
  PiAppleLogo
} from 'vue-icons-plus/pi'

type InstallMethod = {
  type: 'docker' | 'github' | 'download'
  label?: string
  image?: string
  repo?: string
  steps?: string[]
}

type AppItem = {
  name: string
  description?: string
  icon: string
  previews?: string[]

  // EXISTING
  prod: boolean
  comingSoon: boolean
  personalPlatforms?: { name: string; icon: any; rare?: boolean }[]
  technologies: string[]
  languages: string[]
  features?: { icon: any; text: string }[]

  // NEW (non-breaking)
  role?: 'client' | 'server'
  installMethods?: InstallMethod[]
}

const apps: AppItem[] = [
  {
    name: 'Honoka',
    description: 'Optional self-hosted backend for full privacy and control. Used for private media tracking and listing',
    icon: honokaIcon,
    prod: false,
    comingSoon: false,
    role: 'server',
    technologies: ['Node.js', 'Docker'],
    languages: ['CoffeeScript'],
    features: [
      { icon: PiBookOpenDuotone, text: 'Media tracking' },
      { icon: PiCpuDuotone, text: 'Efficient performance' },
      { icon: PiGlobeDuotone, text: 'Remote access' },
    ],
    installMethods: [
      {
        type: 'docker',
        label: 'Quick Start (Docker)',
        image: 'ghcr.io/miyuna/shinobu',
        steps: [
          'Pull image from registry',
          'Run container',
          'Expose port',
          'Connect from client app'
        ]
      },
      {
        type: 'github',
        label: 'Manual Setup',
        repo: 'https://github.com/miyuna/shinobu',
        steps: [
          'Clone repository',
          'Install dependencies',
          'Configure environment',
          'Run server'
        ]
      }
    ]
  },
  {
    name: 'Shinobu',
    description: 'Optional self-hosted backend for full privacy and control. Used for private media streaming, reading and management.',
    icon: shinobuIcon,
    prod: false,
    comingSoon: false,
    role: 'server',
    technologies: ['Node.js', 'Docker'],
    languages: ['JavaScript'],
    installMethods: [
      {
        type: 'docker',
        label: 'Quick Start (Docker)',
        image: 'ghcr.io/miyuna/shinobu',
        steps: [
          'Pull image from registry',
          'Run container',
          'Expose port',
          'Connect from client app'
        ]
      },
      {
        type: 'github',
        label: 'Manual Setup',
        repo: 'https://github.com/miyuna/shinobu',
        steps: [
          'Clone repository',
          'Install dependencies',
          'Configure environment',
          'Run server'
        ]
      }
    ]
  },
  {
    name: 'Shirohana',
    description: 'A modern, with elegant interface, designed for streaming, reading with your personal media server and managing your digital library.',
    icon: shirohanaIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'iOS', icon: PiDeviceMobileDuotone },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'macOS', icon: PiMonitorDuotone },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ['Apache Cordova', "React"],
    languages: ['TypeScript'],
    installMethods: [
      { type: 'download' }
    ],
    features: [
      { icon: PiBookOpenDuotone, text: 'EPUB & PDF support' },
      { icon: PiSparkleDuotone, text: 'Clean reading mode' },
      { icon: PiDownloadSimpleDuotone, text: 'Offline reading' },
      { icon: PiCodeDuotone, text: 'Custom themes' }
    ]
  },

  {
    name: 'Kiirohana',
    description: 'A interactive and more animated UI, designed for streaming, reading with your personal media server and managing your media library.',
    icon: kiirohanaIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [kiirohanaPreview],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'iOS', icon: PiDeviceMobileDuotone },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'macOS', icon: PiMonitorDuotone },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ['Apache Cordova', "React"],
    languages: ['TypeScript'],
    installMethods: [
      { type: 'download' }
    ],
    features: [
      { icon: PiPlayDuotone, text: 'High-speed streaming' },
      { icon: PiCpuDuotone, text: 'Hardware accelerated' },
      { icon: PiGlobeDuotone, text: 'Remote access' },
      { icon: PiSparkleDuotone, text: 'Sleek UI' }
    ]
  },
  {
    name: 'Hanatsuki',
    description: 'Simplified version of Kiirohana, designed for streaming and reading with your personal media server and managing your media library with a more straightforward interface.',
    icon: hanatsukiIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'iOS', icon: PiDeviceMobileDuotone },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'macOS', icon: PiMonitorDuotone },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ['Apache Cordova@6', "Preact"],
    languages: ['JavaScript'],
    installMethods: [
      { type: 'download' }
    ]
  },

  {
    name: 'Sakihana',
    description: 'Old device support version with better compatibility, designed for streaming and reading with your personal media server and managing your media library on older devices.',
    icon: sakihanaIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'iOS', icon: PiDeviceMobileDuotone },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'macOS', icon: PiMonitorDuotone },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ['Apache Cordova@3.6.0', "Vanilla JS"],
    languages: ['JavaScript'],
    installMethods: [
      { type: 'download' }
    ]
  },
  {
    name: 'Kohibana',
    description: 'Old device supporting client, designed for streaming and reading with your personal media server and managing your media library on older devices with more limited resources.',
    icon: kohibanaIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'iOS', icon: PiAppleLogo }, // Menggunakan Apple Logo lebih umum untuk iOS
      { name: 'Windows 8', icon: PiWindowsLogo },
      {
        name: "Amazon Fire OS",
        icon: PiAmazonLogo, // Menggunakan logo Amazon sebagai representasi Fire OS
        rare: true
      },
      {
        name: "BlackBerry OS 6",
        icon: PiDotsNine, // BlackBerry identik dengan grid menu atau "berry" dots
        rare: true
      },
      {
        name: "BlackBerry OS 10",
        icon: PiDevices, // Menunjukkan modernitas BB10 yang sudah full touch
        rare: true
      },
      {
        name: "Windows Phone 7/8",
        icon: PiMicrosoftOutlookLogo, // Bentuk kotak-kotak (tiles) khas Windows Phone
        rare: false
      },
      {
        name: "Tizen",
        icon: PiCircleNotch, // Menyerupai bentuk logo Tizen yang melingkar
        rare: true
      }
    ],
    technologies: ['Apache Cordova@3.2.0', "Vanilla JS"],
    languages: ['JavaScript'],
    installMethods: [
      { type: 'download' }
    ]
  },
  {
    name: "Eclaira",
    description: "Experimental client for streaming and reading with your personal media server and managing your media library on various platforms with a focus on performance and minimalism.",
    icon: upcomingIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'iOS', icon: PiDeviceMobileDuotone },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'macOS', icon: PiMonitorDuotone },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ["Apache Cordova", 'Vue'],
    languages: ['TypeScript'],
    installMethods: [
      { type: 'download' }
    ]
  },
  {
    name: "Chiffon",
    description: "Experimental client for streaming and reading with your personal media server and managing your media library on various platforms with a focus on performance and minimalism.",
    icon: upcomingIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'iOS', icon: PiDeviceMobileDuotone },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'macOS', icon: PiMonitorDuotone },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ["Apache Cordova", 'Vue'],
    languages: ['JavaScript'],
    installMethods: [
      { type: 'download' }
    ]
  },
  {
    name: "Soufflé",
    description: "Experimental client for streaming and reading with your personal media server and managing your media library on various platforms with a focus on performance and minimalism.",
    icon: upcomingIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'iOS', icon: PiDeviceMobileDuotone },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'macOS', icon: PiMonitorDuotone },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ["Apache Cordova", 'Svelte'],
    languages: ['TypeScript'],
    installMethods: [
      { type: 'download' }
    ]
  },
  {
    name: "Madeleine",
    description: "Experimental client for streaming and reading with your personal media server and managing your media library on various platforms with a focus on performance and minimalism.",
    icon: upcomingIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
      { name: 'iOS', icon: PiDeviceMobileDuotone },
      { name: 'Windows', icon: PiWindowsLogo },
      { name: 'macOS', icon: PiMonitorDuotone },
      { name: 'Linux', icon: PiLinuxLogo },
      { name: 'Web', icon: PiGlobeDuotone }
    ],
    technologies: ["Apache Cordova", 'Svelte'],
    languages: ['JavaScript'],
    installMethods: [
      { type: 'download' }
    ]
  },
  {
    name: "Fenrir",
    description: "Experimental client for streaming and reading with your personal media server and managing your media library on various platforms with a focus on performance and minimalism.",
    icon: upcomingIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo },
    ],
    technologies: ['Android', "Jetpack Compose"],
    languages: ['Kotlin'],
    installMethods: [
      { type: 'download' }
    ]
  },
  {
    name: "Phoenix",
    description: "Experimental client for streaming and reading with your personal media server and managing your media library on various platforms with a focus on performance and minimalism.",
    icon: upcomingIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo }
    ],
    technologies: ['Android', "Jetpack Compose"],
    languages: ['Java'],
    installMethods: [
      { type: 'download' }
    ]
  },
  {
    name: "Dragon",
    description: "Experimental client for streaming and reading with your personal media server and managing your media library on various platforms with a focus on performance and minimalism.",
    icon: upcomingIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo }
    ],
    technologies: ['Android'],
    languages: ['Java'],
    installMethods: [
      { type: 'download' }
    ]
  },
  {
    name: "Gryphon",
    description: "Experimental client for streaming and reading with your personal media server and managing your media library on various platforms with a focus on performance and minimalism.",
    icon: upcomingIcon,
    prod: false,
    comingSoon: true,
    role: 'client',
    previews: [],
    personalPlatforms: [
      { name: 'Android', icon: PiAndroidLogo }
    ],
    technologies: ['Android'],
    languages: ['Java'],
    installMethods: [
      { type: 'download' }
    ]
  },
]

const search = ref('')
const selectedPlatform = ref('All')
const selectedTech = ref('All')
const selectedLang = ref('All')

const platformOptions = computed(() => {
  const list = apps.flatMap(i =>
    (i.personalPlatforms ?? []).map(p => p.name)
  )
  return ['All', ...new Set(list)]
})

const techOptions = computed(() => {
  const list = apps.flatMap(i => i.technologies ?? [])
  return ['All', ...new Set(list)]
})

const langOptions = computed(() => {
  const list = apps.flatMap(i => i.languages ?? [])
  return ['All', ...new Set(list)]
})

const filteredApps = computed(() => {
  return apps.filter(app => {
    const byName =
      app.name.toLowerCase().includes(search.value.toLowerCase())

    // ✅ PLATFORM FILTER (skip untuk server)
    const byPlatform =
      selectedPlatform.value === 'All' ||
      app.role === 'server' ||
      (app.personalPlatforms ?? []).some(
        p => p.name === selectedPlatform.value
      )

    const byTech =
      selectedTech.value === 'All' ||
      (app.technologies ?? []).includes(selectedTech.value)

    const byLang =
      selectedLang.value === 'All' ||
      (app.languages ?? []).includes(selectedLang.value)

    return byName && byPlatform && byTech && byLang
  })
})

// ... (import tetap sama)

// --- LOGIKA PAGINATION ---
const currentPage = ref(1)
const itemsPerPage = 6 // Kamu bisa ubah angka ini sesuai kebutuhan

// Reset ke halaman 1 setiap kali filter berubah
import { watch } from 'vue'
watch([search, selectedPlatform, selectedTech, selectedLang], () => {
  currentPage.value = 1
})

const totalPages = computed(() => {
  return Math.ceil(filteredApps.value.length / itemsPerPage)
})

const paginatedApps = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage
  const end = start + itemsPerPage
  return filteredApps.value.slice(start, end)
})

const resetFilter = () => {
  search.value = ''
  selectedPlatform.value = 'All'
  selectedTech.value = 'All'
  selectedLang.value = 'All'
  currentPage.value = 1
}
</script>

<template>
  <div class="min-h-screen bg-slate-950 text-white selection:bg-fuchsia-500/30">

    <!-- HERO -->
    <section class="relative overflow-hidden">
      <img :src="kiirohanaPreview" class="absolute inset-0 w-full h-full object-cover opacity-20" />

      <div class="relative max-w-7xl mx-auto px-4 sm:px-6 py-20 sm:py-28 text-center">
        <p class="text-fuchsia-300 font-semibold tracking-widest uppercase text-xs sm:text-sm">
          Miyuna Ecosystem
        </p>

        <h1 class="text-3xl sm:text-5xl lg:text-6xl font-black mt-4 leading-tight">
          Read. Watch. Own Your Platform.
        </h1>

        <p class="mt-6 text-slate-300 max-w-xl mx-auto">
          Multi-platform private media ecosystem.
          Works instantly out of the box.
          Self-host when you need full control.
        </p>

        <div class="mt-6 sm:mt-8 flex flex-col sm:flex-row gap-3 sm:gap-4 justify-center">
          <a href="#apps" class="px-6 py-3 rounded-xl bg-fuchsia-600 hover:bg-fuchsia-500 text-sm sm:text-base">
            Explore Apps
          </a>

          <a class="px-6 py-3 rounded-xl border border-white/20 text-sm sm:text-base">
            Learn Ecosystem
          </a>
        </div>
      </div>
    </section>

    <section class="max-w-7xl mx-auto px-4 sm:px-6 py-16 sm:py-20">
      <h2 class="text-2xl sm:text-3xl lg:text-4xl font-bold text-center mb-10 sm:mb-12">
        Beautiful Across Devices
      </h2>

      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 sm:gap-6">
        <img :src="kiirohanaPreview" class="rounded-2xl sm:rounded-3xl shadow-xl" />
        <img :src="kiirohanaPreview" class="rounded-2xl sm:rounded-3xl shadow-xl" />
        <img :src="kiirohanaPreview" class="rounded-2xl sm:rounded-3xl shadow-xl" />
      </div>
    </section>

    <!-- QUICK START -->
    <section class="max-w-6xl mx-auto py-16">
      <h2 class="text-2xl font-bold mb-8">Quick Start</h2>

      <div class="grid sm:grid-cols-3 gap-6 text-sm">

        <!-- Step 1 -->
        <div class="bg-black/40 p-6 rounded-xl border border-white/10">
          <div class="text-lg font-semibold mb-2">1. Install App</div>
          <p class="text-white/70">
            Download Shirohana, Kiirohana, or other Miyuna clients.
          </p>
        </div>

        <!-- Step 2 -->
        <div class="bg-black/40 p-6 rounded-xl border border-white/10">
          <div class="text-lg font-semibold mb-2">2. Open & Enjoy</div>
          <p class="text-white/70">
            Launch the app. It works immediately with our public media backend, no setup required.
          </p>
        </div>

        <!-- Step 3 -->
        <div class="bg-black/40 p-6 rounded-xl border border-white/10">
          <div class="text-lg font-semibold mb-2">3. Optional: Self-Host</div>
          <p class="text-white/70">
            Run Shinobu or Honoka for full control, privacy, and your own media backend.
          </p>
        </div>

      </div>
    </section>

    <!-- CONTENT -->
    <main id="apps" class="max-w-7xl mx-auto px-4 sm:px-6 py-10 space-y-8">

      <!-- FILTER PANEL -->
      <section class="rounded-3xl border border-white/10 bg-white/5 backdrop-blur-xl p-4 sm:p-6 shadow-2xl">
        <div class="flex items-center gap-2 mb-5">
          <PiMagnifyingGlass class="w-5 h-5 text-fuchsia-300" />
          <h2 class="text-lg sm:text-xl font-bold">Search & Filters</h2>
        </div>

        <div class="grid grid-cols-1 xs:grid-cols-2 md:grid-cols-4 gap-3 sm:gap-4">
          <input v-model="search" placeholder="Search app..."
            class="px-4 py-3 rounded-xl bg-slate-900 border border-white/10 outline-none focus:border-fuchsia-400 text-sm w-full" />

          <select v-model="selectedPlatform"
            class="px-4 py-3 rounded-xl bg-slate-900 border border-white/10 text-sm w-full">
            <option v-for="item in platformOptions" :key="item" :value="item">
              {{ item }}
            </option>
          </select>

          <select v-model="selectedTech"
            class="px-4 py-3 rounded-xl bg-slate-900 border border-white/10 text-sm w-full">
            <option v-for="item in techOptions" :key="item" :value="item">
              {{ item }}
            </option>
          </select>

          <select v-model="selectedLang"
            class="px-4 py-3 rounded-xl bg-slate-900 border border-white/10 text-sm w-full">
            <option v-for="item in langOptions" :key="item" :value="item">
              {{ item }}
            </option>
          </select>
        </div>

        <div class="flex flex-col xs:flex-row justify-between items-center mt-5 gap-4">
          <p class="text-sm text-slate-400">
            Showing {{ filteredApps.length }} app(s)
          </p>

          <button @click="resetFilter"
            class="w-full xs:w-auto inline-flex items-center justify-center gap-2 px-6 py-2 rounded-xl bg-fuchsia-600 hover:bg-fuchsia-500 transition">
            <PiX class="w-4 h-4" />
            Reset
          </button>
        </div>
      </section>

      <!-- APP LIST -->
      <section class="rounded-3xl border border-white/10 bg-white/5 backdrop-blur-xl p-4 sm:p-6">
        <div class="flex items-center gap-2 mb-6">
          <PiDownloadSimpleDuotone class="w-5 h-5 text-fuchsia-300" />
          <h2 class="text-xl sm:text-2xl font-bold">Applications</h2>
        </div>

        <!-- UBAH filteredApps MENJADI paginatedApps -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          <CardApp v-for="app in paginatedApps" :key="app.name" :appName="app.name" :appDescription="app.description"
            :appIcon="app.icon" :previews="app.previews" :production="app.prod"
            :personalPlatforms="app.personalPlatforms" :comingSoon="app.comingSoon" :features="app.features"
            :technologies="app.technologies" :languages="app.languages" :role="app.role"
            :installMethods="app.installMethods" />
        </div>

        <div v-if="filteredApps.length === 0" class="text-center py-12 text-slate-400">
          No applications found.
        </div>

        <!-- TAMBAHKAN NAVIGASI PAGINATION DISINI -->
        <div v-if="totalPages > 1" class="flex justify-center items-center gap-4 mt-10 pt-6 border-t border-white/5">
          <button @click="currentPage--" :disabled="currentPage === 1"
            class="px-4 py-2 rounded-lg bg-white/5 border border-white/10 disabled:opacity-30 disabled:cursor-not-allowed hover:bg-white/10 transition text-sm">
            Previous
          </button>

          <div class="flex gap-2">
            <button v-for="page in totalPages" :key="page" @click="currentPage = page" :class="[
              'w-10 h-10 rounded-lg border transition text-sm font-medium',
              currentPage === page
                ? 'bg-fuchsia-600 border-fuchsia-500 text-white'
                : 'bg-white/5 border-white/10 text-slate-400 hover:border-fuchsia-500/50'
            ]">
              {{ page }}
            </button>
          </div>

          <button @click="currentPage++" :disabled="currentPage === totalPages"
            class="px-4 py-2 rounded-lg bg-white/5 border border-white/10 disabled:opacity-30 disabled:cursor-not-allowed hover:bg-white/10 transition text-sm">
            Next
          </button>
        </div>
      </section>
    </main>

    <!-- FOOTER -->
    <footer class="border-t border-white/10 mt-10">
      <div class="max-w-7xl mx-auto px-6 py-8 text-sm text-slate-400 flex flex-col md:flex-row justify-between gap-4">
        <p>© {{ new Date().getFullYear() }} Miyuna Developer</p>
        <p>Modern UI • Responsive • Fast Filtering</p>
      </div>
    </footer>
  </div>
</template>