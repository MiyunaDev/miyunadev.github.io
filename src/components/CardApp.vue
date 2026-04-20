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
  PiCheckCircleDuotone
} from "vue-icons-plus/pi"

const props = defineProps<{
  appName: string
  appDescription?: string
  previews: string[]
  production: boolean
  personalPlatforms: { name: string; icon: any }[]
  comingSoon?: boolean
  appIcon: string
  features?: { icon: any; text: string }[]
  technologies?: string[]
  languages?: string[]
}>()

const releases = ref<any[]>([])
const downloads = ref(0)
const zoomedImage = ref<string | null>(null)
const loading = ref(true)
const expanded = ref(false)
const detectedOS = ref("unknown")
const copied = ref(false)

const timeAgo = (dateString: string) => {
  const date = new Date(dateString)
  const now = new Date()
  const diffInSeconds = Math.floor((now.getTime() - date.getTime()) / 1000)

  if (diffInSeconds < 60) return "just now"
  if (diffInSeconds < 3600) return `${Math.floor(diffInSeconds / 60)}m ago`
  if (diffInSeconds < 86400) return `${Math.floor(diffInSeconds / 3600)}h ago`
  if (diffInSeconds < 2592000) return `${Math.floor(diffInSeconds / 86400)}d ago`
  return date.toLocaleDateString()
}

const lastUpdated = computed(() => {
  if (!latestRelease.value?.published_at) return null
  return timeAgo(latestRelease.value.published_at)
})

const copyToClipboard = async (text: string) => {
  try {
    await navigator.clipboard.writeText(text)
    copied.value = true
    setTimeout(() => (copied.value = false), 2000)
  } catch (err) {
    console.error("Failed to copy", err)
  }
}

onMounted(() => {
  const ua = navigator.userAgent.toLowerCase()

  if (ua.includes("windows")) detectedOS.value = "windows"
  else if (ua.includes("android")) detectedOS.value = "android"
  else if (ua.includes("iphone") || ua.includes("ipad")) detectedOS.value = "ios"
  else if (ua.includes("mac")) detectedOS.value = "mac"
  else if (ua.includes("linux")) detectedOS.value = "linux"
})

onMounted(async () => {
  const cacheKey = `releases-${props.appName}`
  const cachedData = localStorage.getItem(cacheKey)
  
  if (cachedData) {
    const { data, timestamp } = JSON.parse(cachedData)
    // Cache for 30 minutes
    if (Date.now() - timestamp < 30 * 60 * 1000) {
      releases.value = data
      updateDownloadCount(data)
      loading.value = false
      return
    }
  }

  try {
    const res = await axios.get(
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

function updateDownloadCount(data: any[]) {
  downloads.value = 0
  for (const rel of data) {
    for (const asset of rel.assets) {
      downloads.value += asset.download_count
    }
  }
}

const latestRelease = computed(() => releases.value?.[0] || null)

function getIcon(filename: string) {
  const f = filename.toLowerCase()
  // ---------- Android ----------
  if (f.endsWith(".apk")) { return { icon: PiAndroidLogo, label: "Android APK Binary" } }
  if (f.endsWith(".aab")) { return { icon: PiAndroidLogo, label: "Android App Bundle" } }

  // ---------- iOS ----------
  if (f.endsWith(".ipa")) { return { icon: PiDeviceMobileDuotone, label: "iOS IPA Package" } }

  // ---------- Windows ----------
  if (f.endsWith(".exe")) { return { icon: PiWindowsLogo, label: "Windows Installer (.exe)" } }
  if (f.endsWith(".msi")) { return { icon: PiWindowsLogo, label: "Windows MSI Installer" } }
  if (f.endsWith(".zip") && f.includes("windows")) { return { icon: PiWindowsLogo, label: "Windows Portable Binary" } }

  // ---------- Linux ----------
  if (f.endsWith(".deb")) { return { icon: PiLinuxLogo, label: "Ubuntu / Debian Package" } }
  if (f.endsWith(".rpm")) { return { icon: PiLinuxLogo, label: "Fedora / RHEL RPM Package" } }
  if (f.endsWith(".pacman")) { return { icon: PiLinuxLogo, label: "Arch Linux Package" } }
  if (f.endsWith(".snap")) { return { icon: PiLinuxLogo, label: "Snap Binary Package" } }
  if (f.endsWith(".flatpak")) { return { icon: PiLinuxLogo, label: "Flatpak Package" } }
  if (f.endsWith(".appimage")) { return { icon: PiLinuxLogo, label: "Portable Linux Binary" } }
  if (f.endsWith(".tar.gz") || f.endsWith(".tar.xz")) { return { icon: PiLinuxLogo, label: "Linux Binary Archive" } }

  // ---------- macOS ----------
  if (f.endsWith(".dmg")) { return { icon: PiMonitorDuotone, label: "macOS Disk Image (.dmg)" } }
  if (f.endsWith(".pkg")) { return { icon: PiMonitorDuotone, label: "macOS Installer Package" } }
  if (f.endsWith(".app.zip")) { return { icon: PiMonitorDuotone, label: "macOS App Binary" } }

  // ---------- fallback ----------
  return { icon: PiDownloadDuotone, label: "Download File" }
}

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

const matchedAsset = computed(() => {
  const assets = latestRelease.value?.assets || []
  if (!assets || assets.length === 0) return null

  const f = (ext: string) => assets.find((a: any) => a.name.toLowerCase().endsWith(ext))

  if (detectedOS.value === "windows") return f(".exe") || f(".msi")
  if (detectedOS.value === "android") return f(".apk") || f(".aab")
  if (detectedOS.value === "ios") return f(".ipa")
  if (detectedOS.value === "mac") return f(".dmg") || f(".pkg") || f(".app.zip")
  if (detectedOS.value === "linux") return f(".deb") || f(".appimage") || f(".rpm") || f(".pacman")

  return null
})

const allAssets = computed(() => latestRelease.value?.assets || [])

const otherAssets = computed(() => {
  const assets = allAssets.value
  if (!matchedAsset.value) return assets
  return assets.filter((a: any) => a.id !== matchedAsset.value.id)
})
</script>

<template>
  <div
    class="group relative overflow-hidden rounded-3xl border border-white/10 bg-gradient-to-br from-slate-900 via-slate-900 to-fuchsia-950 shadow-xl transition hover:-translate-y-1">

    <div class="relative p-4 sm:p-6 space-y-6">

      <!-- COMING SOON -->
      <div v-if="comingSoon && !allAssets.length" class="rounded-2xl border border-yellow-500/20 bg-yellow-500/10 p-4 flex gap-3">
        <PiLockDuotone class="w-5 h-5 text-yellow-300 mt-0.5" />
        <div>
          <p class="font-semibold text-yellow-200">Coming Soon</p>
          <p class="text-sm text-yellow-100/70">This app is not released yet.</p>
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
            <div class="flex gap-1">
              <component v-for="p in personalPlatforms" :key="p.name" :is="p.icon"
                class="w-4 h-4 text-slate-500" :title="p.name" />
            </div>
          </div>

          <p class="text-sm text-slate-400">
            {{ appDescription || 'Miyuna Ecosystem App' }}
          </p>

          <div class="flex flex-wrap gap-3 mt-2 text-[11px] text-slate-300">
            <span class="flex items-center gap-1 bg-white/5 px-2 py-0.5 rounded-full">
              <PiArrowCircleDownDuotone class="w-3.5 h-3.5 text-fuchsia-400" />
              {{ downloads.toLocaleString() }}
            </span>

            <span v-if="latestRelease?.tag_name"
              @click="copyToClipboard(latestRelease.tag_name)"
              class="flex items-center gap-1 bg-white/5 px-2 py-0.5 rounded-full cursor-pointer hover:bg-white/10 transition-colors group/tag">
              <PiTagDuotone class="w-3.5 h-3.5 text-cyan-400" />
              {{ latestRelease.tag_name }}
              <component :is="copied ? PiCheckCircleDuotone : PiCopyDuotone"
                class="w-3 h-3 opacity-0 group-hover/tag:opacity-100 transition-opacity" />
            </span>

            <span v-if="lastUpdated" class="flex items-center gap-1 bg-white/5 px-2 py-0.5 rounded-full">
              <PiCalendarDuotone class="w-3.5 h-3.5 text-amber-400" />
              Updated {{ lastUpdated }}
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
      <div v-if="previews.length" class="grid grid-cols-1 sm:grid-cols-2 gap-3">
        <img v-for="(img, i) in previews" :key="i" :src="img" @click="zoomedImage = img"
          class="h-28 sm:h-32 w-full object-cover rounded-2xl border border-white/10 cursor-zoom-in hover:scale-[1.02] transition" />
      </div>

      <!-- FEATURES -->
      <div class="grid grid-cols-2 gap-y-2 gap-x-4 text-xs text-slate-300">
        <template v-if="features && features.length">
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
            <PiDeviceMobileDuotone class="w-3.5 h-3.5 text-fuchsia-400 shrink-0" /> <span class="truncate">Mobile ready</span>
          </div>
          <div class="flex items-center gap-2 min-w-0">
            <PiGlobeDuotone class="w-3.5 h-3.5 text-fuchsia-400 shrink-0" /> <span class="truncate">Multi-language</span>
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

      <!-- DOWNLOAD -->
      <div v-if="allAssets.length" class="space-y-3">

        <a v-if="matchedAsset" :href="matchedAsset.browser_download_url" target="_blank"
          class="flex flex-col sm:flex-row items-center justify-center gap-2 sm:gap-3 bg-fuchsia-600 hover:bg-fuchsia-500 rounded-2xl px-5 py-4 font-semibold text-center transition-colors">
          <component :is="getIcon(matchedAsset.name).icon" class="w-5 h-5" />
          Download {{ getIcon(matchedAsset.name).label }}
          <span class="text-xs opacity-80">
            {{ formatBytes(matchedAsset.size) }}
          </span>
        </a>

        <button v-if="otherAssets.length" @click="expanded = !expanded"
          class="w-full py-3 rounded-2xl bg-white/5 hover:bg-white/10 border border-white/10 transition-colors">
          {{ expanded ? "Hide" : (matchedAsset ? "Other Downloads" : "Available Downloads") }}
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
                <span class="text-[9px] w-fit px-1.5 py-0.5 rounded bg-white/5 text-slate-400 group-hover/item:text-slate-300 group-hover/item:bg-white/10 transition-colors whitespace-nowrap shrink-0">
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

      <!-- LOADING -->
      <div v-if="loading" class="text-center text-sm text-slate-500">
        Loading...
      </div>
    </div>

    <!-- IMAGE MODAL FIXED -->
    <Teleport to="body">
      <div v-if="zoomedImage"
        class="fixed inset-0 z-[9999] flex items-center justify-center bg-black/90 backdrop-blur-sm"
        @click.self="zoomedImage = null">
        <!-- close button -->
        <button @click="zoomedImage = null"
          class="absolute top-5 right-5 z-[10000] p-2 rounded-xl bg-white/10 hover:bg-white/20">
          <PiX class="w-5 h-5 text-white" />
        </button>

        <!-- image -->
        <img :src="zoomedImage" class="max-w-[95vw] max-h-[90vh] rounded-2xl shadow-2xl object-contain"
          draggable="false" />
      </div>
    </Teleport>
  </div>
</template>