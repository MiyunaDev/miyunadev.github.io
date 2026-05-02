<script lang="ts" setup>
import axios from "axios"
import { ref, onMounted, computed } from "vue"

import {
  PiAndroidLogo,
  PiWindowsLogo,
  PiLinuxLogo,
  PiDownloadDuotone,
  PiArrowCircleDownDuotone,
  PiTagDuotone,
  PiWarningCircleDuotone,
  PiMonitorDuotone,
  PiDeviceMobileDuotone,
  PiGlobeDuotone,
  PiBookOpenDuotone,
  PiPlayDuotone,
  PiLockDuotone,
  PiX,
  PiCpuDuotone,
  PiCalendarDuotone,
  PiCopyDuotone,
  PiCheckCircleDuotone,
  PiCodeDuotone,
  PiCaretDownDuotone, // ✅ Ditambahkan untuk fitur baru
  PiWarningDuotone
} from "vue-icons-plus/pi"

/* ================= INTERFACES ================= */
interface GithubAsset {
  id: number
  name: string
  size: number
  download_count: number
  browser_download_url: string
}

interface GithubRelease {
  tag_name: string
  published_at: string
  assets: GithubAsset[]
}

/* ================= PROPS ================= */
const props = withDefaults(defineProps<{
  appName: string
  appDescription?: string
  previews?: string[]
  production?: boolean
  personalPlatforms?: { name: string; icon: any }[]
  comingSoon?: boolean
  appIcon: string
  features?: { icon: any; text: string }[]
  technologies?: string[]
  languages?: string[]

  // NEW: Tambahan fitur Client/Server
  role?: 'client' | 'server'
  installMethods?: {
    type: 'docker' | 'github' | 'download'
    label?: string
    image?: string
    repo?: string
    steps?: string[]
  }[]
}>(), {
  previews: () => [],
  production: true,
  personalPlatforms: () => [],
  comingSoon: false,
  features: () => [],
  technologies: () => [],
  languages: () => [],
  role: 'client',
  installMethods: () => []
})

/* ================= STATE ================= */
const releases = ref<GithubRelease[]>([])
const downloads = ref(0)
const zoomedImage = ref<string | null>(null)
const loading = ref(true)
const expanded = ref(false)
const expandedInstall = ref(false) // State baru untuk server
const detectedOS = ref<"windows" | "android" | "ios" | "mac" | "linux" | "unknown">("unknown")
const copied = ref(false)

/* ================= ROLE ================= */
const isServer = computed(() => props.role === 'server')
const isClient = computed(() => props.role !== 'server')

/* ================= TIME & FORMAT ================= */
const timeAgo = (dateString: string) => {
  const date = new Date(dateString)
  const diffInSeconds = Math.floor((Date.now() - date.getTime()) / 1000)

  if (diffInSeconds < 60) return "just now"
  if (diffInSeconds < 3600) return `${Math.floor(diffInSeconds / 60)}m ago`
  if (diffInSeconds < 86400) return `${Math.floor(diffInSeconds / 3600)}h ago`
  if (diffInSeconds < 2592000) return `${Math.floor(diffInSeconds / 86400)}d ago`
  return date.toLocaleDateString()
}

const latestRelease = computed(() => releases.value?.[0] || null)

const lastUpdated = computed(() => {
  if (!latestRelease.value?.published_at) return null
  return timeAgo(latestRelease.value.published_at)
})

const formatBytes = (bytes: number) => {
  if (!bytes && bytes !== 0) return "Unknown"
  const units = ["B", "KB", "MB", "GB"]
  let i = 0
  let size = bytes

  while (size >= 1024 && i < units.length - 1) {
    size /= 1024
    i++
  }

  return `${size.toFixed(i === 0 ? 0 : 2)} ${units[i]}`
}

/* ================= COPY ================= */
const copyToClipboard = async (text: string) => {
  try {
    await navigator.clipboard.writeText(text)
    copied.value = true
    setTimeout(() => (copied.value = false), 2000)
  } catch (err) {
    console.error("Failed to copy", err)
  }
}

/* ================= OS DETECT ================= */
onMounted(() => {
  const ua = navigator.userAgent.toLowerCase()

  if (ua.includes("windows")) detectedOS.value = "windows"
  else if (ua.includes("android")) detectedOS.value = "android"
  else if (ua.includes("iphone") || ua.includes("ipad")) detectedOS.value = "ios"
  else if (ua.includes("mac")) detectedOS.value = "mac"
  else if (ua.includes("linux")) detectedOS.value = "linux"
})

/* ================= FETCH & CACHE (RESTORED) ================= */
onMounted(async () => {
  const cacheKey = `releases-${props.appName}`
  const cachedData = localStorage.getItem(cacheKey)

  if (cachedData) {
    try {
      const { data, timestamp } = JSON.parse(cachedData)
      if (Date.now() - timestamp < 60 * 1000) {
        releases.value = data
        updateDownloadCount(data)
        loading.value = false
        return
      }
    } catch (e) {
      console.warn("Invalid cache data, fetching fresh data...")
      localStorage.removeItem(cacheKey)
    }
  }

  try {
    const res = await axios.get<GithubRelease[]>(
      `https://api.github.com/repos/MiyunaDev/${props.appName}/releases`
    )

    releases.value = res.data
    updateDownloadCount(res.data)

    localStorage.setItem(cacheKey, JSON.stringify({
      data: res.data,
      timestamp: Date.now()
    }))
  } catch (err) {
    console.error("Failed fetching releases", err)
  } finally {
    loading.value = false
  }
})

function updateDownloadCount(data: GithubRelease[]) {
  downloads.value = data.reduce((total, rel) => {
    return total + rel.assets.reduce((assetTotal, asset) => assetTotal + asset.download_count, 0)
  }, 0)
}

/* ================= FILE LOGIC (RESTORED LABELS & SIZE) ================= */
const allAssets = computed(() => latestRelease.value?.assets || [])

const matchedAsset = computed(() => {
  const assets = allAssets.value
  if (!assets.length) return null

  const f = (ext: string) => assets.find(a => a.name.toLowerCase().endsWith(ext))

  if (detectedOS.value === "windows") return f(".exe") || f(".msi")
  if (detectedOS.value === "android") return f(".apk") || f(".aab")
  if (detectedOS.value === "ios") return f(".ipa")
  if (detectedOS.value === "mac") return f(".dmg") || f(".pkg") || f(".app.zip")
  if (detectedOS.value === "linux") return f(".deb") || f(".appimage") || f(".rpm") || f(".pacman")

  return null
})

const otherAssets = computed(() => {
  const assets = allAssets.value
  if (!matchedAsset.value) return assets
  return assets.filter(a => a.id !== matchedAsset.value?.id)
})

function getIcon(filename: string) {
  const f = filename.toLowerCase()
  if (f.endsWith(".apk")) return { icon: PiAndroidLogo, label: "Android APK" }
  if (f.endsWith(".aab")) return { icon: PiAndroidLogo, label: "Android App Bundle" }
  if (f.endsWith(".ipa")) return { icon: PiDeviceMobileDuotone, label: "iOS Package" }
  if (f.endsWith(".exe")) return { icon: PiWindowsLogo, label: "Windows Installer" }
  if (f.endsWith(".msi")) return { icon: PiWindowsLogo, label: "Windows MSI" }
  if (f.endsWith(".zip") && f.includes("windows")) return { icon: PiWindowsLogo, label: "Windows Portable" }
  if (f.endsWith(".deb")) return { icon: PiLinuxLogo, label: "Ubuntu / Debian" }
  if (f.endsWith(".rpm")) return { icon: PiLinuxLogo, label: "Fedora / RHEL" }
  if (f.endsWith(".pacman")) return { icon: PiLinuxLogo, label: "Arch Linux" }
  if (f.endsWith(".snap") || f.endsWith(".flatpak")) return { icon: PiLinuxLogo, label: "Linux Package" }
  if (f.endsWith(".appimage")) return { icon: PiLinuxLogo, label: "Portable Linux" }
  if (f.endsWith(".tar.gz") || f.endsWith(".tar.xz")) return { icon: PiLinuxLogo, label: "Linux Archive" }
  if (f.endsWith(".dmg")) return { icon: PiMonitorDuotone, label: "macOS DMG" }
  if (f.endsWith(".pkg") || f.endsWith(".app.zip")) return { icon: PiMonitorDuotone, label: "macOS Binary" }

  return { icon: PiDownloadDuotone, label: "Download File" }
}
</script>

<template>
  <div
    class="group relative overflow-hidden rounded-3xl border border-white/10 bg-linear-to-brrom-slate-900 via-slate-900 to-fuchsia-950 shadow-xl transition hover:-translate-y-1">

    <div class="relative p-4 sm:p-6 space-y-6">

      <!-- COMING SOON -->
      <div v-if="comingSoon && !allAssets.length"
        class="rounded-2xl border border-yellow-500/20 bg-yellow-500/10 p-4 flex gap-3">
        <PiLockDuotone class="w-5 h-5 text-yellow-300 mt-0.5" />
        <div>
          <p class="font-semibold text-yellow-200">Coming Soon</p>
          <p class="text-sm text-yellow-100/70">This app is not released yet.</p>
        </div>
      </div>
      <div v-else-if="production === false"
        class="rounded-2xl border border-slate-500/20 bg-slate-500/10 p-4 flex gap-3">
        <PiWarningDuotone class="w-5 h-5 text-slate-400 mt-0.5" />
        <div>
          <p class="font-semibold text-slate-300">In Development</p>
          <p class="text-sm text-slate-300/70">This app is currently in development and may not be fully functional.</p>
        </div>
      </div>

      <!-- HEADER -->
      <div class="flex flex-col sm:flex-row sm:items-start gap-4">
        <img :src="appIcon" class="w-14 h-14 sm:w-16 sm:h-16 rounded-2xl border border-white/10 shrink-0" />

        <div class="flex-1 min-w-0">
          <div class="flex items-center justify-between gap-2">
            <h2 class="text-xl sm:text-2xl font-bold text-white truncate">
              {{ appName }}
            </h2>
            <!-- Platform Badges -->
            <div v-if="isClient && personalPlatforms?.length" class="flex gap-1 shrink-0">
              <component v-for="p in personalPlatforms" :key="p.name" :is="p.icon" class="w-4 h-4 text-slate-500"
                :title="p.name" />
            </div>
          </div>

          <div class="flex items-center gap-2 mb-2">
            <span v-if="role === 'client'"
              class="text-xs px-2 py-1 rounded-lg bg-green-500/10 text-green-400 border border-green-500/20">
              App
            </span>

            <span v-else
              class="text-xs px-2 py-1 rounded-lg bg-yellow-500/10 text-yellow-400 border border-yellow-500/20">
              Self-Hosted
            </span>
          </div>

          <p class="text-sm text-slate-400">
            {{ appDescription || 'Miyuna Ecosystem App' }}
          </p>

          <!-- Badges (Download, Tag, Date) - Only for Client / Apps with Releases -->
          <div v-if="isClient" class="flex flex-wrap gap-3 mt-2 text-[11px] text-slate-300">
            <span class="flex items-center gap-1 bg-white/5 px-2 py-0.5 rounded-full">
              <PiArrowCircleDownDuotone class="w-3.5 h-3.5 text-fuchsia-400" />
              {{ downloads.toLocaleString() }}
            </span>

            <span v-if="latestRelease?.tag_name" @click="copyToClipboard(latestRelease.tag_name)"
              class="flex items-center gap-1 bg-white/5 px-2 py-0.5 rounded-full cursor-pointer hover:bg-white/10 transition-colors group/tag">
              <PiTagDuotone class="w-3.5 h-3.5 text-cyan-400" />
              {{ latestRelease.tag_name }}
              <component :is="copied ? PiCheckCircleDuotone : PiCopyDuotone"
                class="w-3 h-3 opacity-0 group-hover/tag:opacity-100 transition-opacity" />
            </span>

            <span v-if="lastUpdated" class="flex items-center gap-1 bg-white/5 px-2 py-0.5 rounded-full">
              <PiCalendarDuotone class="w-3.5 h-3.5 text-amber-400" />
              {{ lastUpdated }}
            </span>
          </div>

          <!-- Docs Button -->
          <a :href="`https://github.com/MiyunaDev/${appName}`" target="_blank"
            class="inline-flex mt-3 items-center gap-2 px-3 py-1.5 rounded-xl bg-white/5 hover:bg-white/10 border border-white/10 text-xs transition-colors">
            <PiBookOpenDuotone class="w-3.5 h-3.5" />
            Source Code & Docs
          </a>
        </div>
      </div>

      <!-- PREVIEW -->
      <div v-if="previews?.length" class="grid grid-cols-1 sm:grid-cols-2 gap-3">
        <img v-for="(img, i) in previews" :key="i" :src="img" @click="zoomedImage = img"
          class="h-28 sm:h-32 w-full object-cover rounded-2xl border border-white/10 cursor-zoom-in hover:scale-[1.02] transition" />
      </div>

      <!-- FEATURES -->
      <div class="grid grid-cols-2 gap-y-2 gap-x-4 text-xs text-slate-300">
        <template v-if="features?.length">
          <div v-for="f in features" :key="f.text" class="flex items-center gap-2 min-w-0">
            <component :is="f.icon" class="w-3.5 h-3.5 text-fuchsia-400 shrink-0" />
            <span class="truncate">{{ f.text }}</span>
          </div>
        </template>
        <template v-else>
          <div class="flex items-center gap-2 min-w-0">
            <PiPlayDuotone class="w-3.5 h-3.5 text-fuchsia-400 shrink-0" /> <span class="truncate">Media reader</span>
          </div>
          <div class="flex items-center gap-2 min-w-0">
            <PiBookOpenDuotone class="w-3.5 h-3.5 text-fuchsia-400 shrink-0" /> <span class="truncate">EPUB & PDF</span>
          </div>
          <div class="flex items-center gap-2 min-w-0">
            <PiDeviceMobileDuotone class="w-3.5 h-3.5 text-fuchsia-400 shrink-0" /> <span class="truncate">Mobile
              ready</span>
          </div>
          <div class="flex items-center gap-2 min-w-0">
            <PiGlobeDuotone class="w-3.5 h-3.5 text-fuchsia-400 shrink-0" /> <span
              class="truncate">Multi-language</span>
          </div>
        </template>
      </div>

      <!-- TECH & LANG -->
      <div v-if="technologies?.length || languages?.length" class="grid grid-cols-2 gap-4 pt-4 border-t border-white/5">
        <div v-if="technologies?.length" class="min-w-0">
          <p class="text-[10px] font-bold uppercase tracking-widest text-slate-500 flex items-center gap-1.5 mb-2">
            <PiCpuDuotone class="w-3 h-3" /> Stack
          </p>
          <div class="flex flex-wrap gap-1.5">
            <span v-for="tech in technologies" :key="tech"
              class="px-2 py-0.5 rounded-lg bg-fuchsia-500/10 text-fuchsia-300 text-[10px] font-bold border border-fuchsia-500/10 truncate max-w-full">
              {{ tech }}
            </span>
          </div>
        </div>

        <div v-if="languages?.length" class="min-w-0">
          <p class="text-[10px] font-bold uppercase tracking-widest text-slate-500 flex items-center gap-1.5 mb-2">
            <PiCodeDuotone class="w-3 h-3" /> Core
          </p>
          <div class="flex flex-wrap gap-1.5">
            <span v-for="lang in languages" :key="lang"
              class="px-2 py-0.5 rounded-lg bg-cyan-500/10 text-cyan-300 text-[10px] font-bold border border-cyan-500/10 truncate max-w-full">
              {{ lang }}
            </span>
          </div>
        </div>
      </div>

      <!-- CLIENT DOWNLOAD LOGIC (RESTORED UX & FULL CAPABILITY) -->
      <div v-if="isClient && allAssets.length" class="space-y-3">
        <a v-if="matchedAsset" :href="matchedAsset.browser_download_url" target="_blank"
          class="flex flex-col sm:flex-row items-center justify-center gap-2 sm:gap-3 bg-fuchsia-600 hover:bg-fuchsia-500 rounded-2xl px-5 py-4 font-semibold text-center transition-colors">
          <component :is="getIcon(matchedAsset.name).icon" class="w-5 h-5" />
          Download {{ getIcon(matchedAsset.name).label }}
          <span class="text-xs opacity-80">
            {{ formatBytes(matchedAsset.size) }}
          </span>
        </a>

        <button v-if="otherAssets.length" @click="expanded = !expanded"
          class="w-full py-3 rounded-2xl bg-white/5 hover:bg-white/10 border border-white/10 flex justify-between items-center px-4 transition-colors">
          <span class="font-medium text-slate-200">{{ expanded ? "Hide" : (matchedAsset ? "Other Downloads" : "Available Downloads") }}</span>
          <PiCaretDownDuotone class="w-5 h-5 transition-transform duration-300"
            :class="expanded ? 'rotate-180' : ''" />
        </button>

        <div v-if="expanded" class="flex flex-col gap-2.5">
          <a v-for="asset in otherAssets" :key="asset.id" :href="asset.browser_download_url" target="_blank"
            class="flex items-start gap-3 p-3 rounded-2xl bg-white/5 border border-white/10 hover:bg-white/10 group/item transition-colors min-w-0 w-full">
            <component :is="getIcon(asset.name).icon" class="w-5 h-5 text-fuchsia-300 shrink-0 mt-0.5" />

            <div class="min-w-0 flex-1">
              <div class="flex flex-col xs:flex-row xs:items-center justify-between gap-1">
                <p class="text-sm font-medium truncate">
                  {{ getIcon(asset.name).label }}
                </p>
                <span
                  class="text-[9px] w-fit px-1.5 py-0.5 rounded bg-white/5 text-slate-400 group-hover/item:text-slate-300 group-hover/item:bg-white/10 transition-colors whitespace-nowrap shrink-0">
                  {{ formatBytes(asset.size) }}
                </span>
              </div>
              <p class="text-[10px] text-slate-500 truncate mt-0.5 opacity-70">
                {{ asset.name }}
              </p>
            </div>
          </a>
        </div>
      </div>

      <!-- SERVER DEPLOYMENT LOGIC (NEW, STYLED TO MATCH) -->
      <div v-if="isServer && installMethods?.length" class="space-y-3">
        <button @click="expandedInstall = !expandedInstall"
          class="w-full py-3 rounded-2xl bg-white/5 hover:bg-white/10 border border-white/10 flex justify-between items-center px-4 transition-colors">
          <span class="font-medium text-slate-200">Deployment Methods</span>
          <PiCaretDownDuotone class="w-5 h-5 transition-transform duration-300"
            :class="expandedInstall ? 'rotate-180' : ''" />
        </button>

        <div v-if="expandedInstall" class="flex flex-col gap-3">
          <div v-for="(method, idx) in installMethods" :key="idx"
            class="rounded-2xl border border-white/10 bg-white/5 p-4">

            <p class="text-sm font-semibold mb-3 text-fuchsia-300 flex items-center gap-2">
              <PiCpuDuotone v-if="method.type === 'docker'" class="w-4 h-4" />
              <PiBookOpenDuotone v-else class="w-4 h-4" />
              {{ method.label || (method.type === 'docker' ? 'Docker Container' : 'GitHub Repository') }}
            </p>

            <div v-if="method.type === 'docker'" class="relative">
              <code
                class="block bg-black/40 px-3 py-2.5 rounded-xl text-xs text-slate-300 font-mono overflow-x-auto border border-white/5">
          docker run {{ method.image }}
        </code>
              <!-- Menambahkan tombol copy untuk docker command -->
              <button @click="copyToClipboard(`docker run ${method.image}`)"
                class="absolute right-2 top-2 p-1.5 bg-white/10 hover:bg-white/20 rounded-lg transition-colors group/copyBtn"
                title="Copy Command">
                <component :is="copied ? PiCheckCircleDuotone : PiCopyDuotone" class="w-3.5 h-3.5 text-slate-300" />
              </button>
            </div>

            <a v-if="method.type === 'github'" :href="method.repo" target="_blank"
              class="inline-flex items-center gap-2 px-3 py-2 rounded-xl bg-white/5 hover:bg-white/10 border border-white/10 text-xs transition-colors text-slate-200">
              <PiBookOpenDuotone class="w-3.5 h-3.5" />
              Open Repository
            </a>

            <ul v-if="method.steps && method.steps.length" class="mt-3 text-xs text-slate-400 space-y-1.5 list-inside">
              <li v-for="(s, i) in method.steps" :key="i" class="flex gap-2">
                <span class="text-slate-500 font-mono shrink-0">{{ i + 1 }}.</span>
                <span>{{ s }}</span>
              </li>
            </ul>
          </div>
        </div>
      </div>

      <!-- LOADING -->
      <div v-if="loading" class="text-center text-sm text-slate-500">
        Loading...
      </div>
    </div>

    <!-- IMAGE MODAL FIXED -->
    <Teleport to="body">
      <div v-if="zoomedImage" class="fixed inset-0 z-9999 flex items-center justify-center bg-black/90 backdrop-blur-sm"
        @click.self="zoomedImage = null">
        <!-- close button -->
        <button @click="zoomedImage = null"
          class="absolute top-5 right-5 z-10000 p-2 rounded-xl bg-white/10 hover:bg-white/20 transition-colors">
          <PiX class="w-5 h-5 text-white" />
        </button>

        <!-- image -->
        <img :src="zoomedImage" class="max-w-[95vw] max-h-[90vh] rounded-2xl shadow-2xl object-contain"
          draggable="false" />
      </div>
    </Teleport>
  </div>
</template>