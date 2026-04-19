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
  PiSparkleDuotone,
  PiCpuDuotone
} from "vue-icons-plus/pi"

const props = defineProps<{
  appName: string
  previews: string[]
  production: boolean
  personalPlatforms: { name: string; icon: any }[]
  comingSoon?: boolean
  appIcon: string
}>()

const releases = ref<any[]>([])
const downloads = ref(0)
const zoomedImage = ref<string | null>(null)
const loading = ref(true)
const expanded = ref(false)
const detectedOS = ref("unknown")

onMounted(() => {
  const ua = navigator.userAgent.toLowerCase()

  if (ua.includes("windows")) detectedOS.value = "windows"
  else if (ua.includes("android")) detectedOS.value = "android"
  else if (ua.includes("iphone") || ua.includes("ipad"))
    detectedOS.value = "ios"
  else if (ua.includes("mac"))
    detectedOS.value = "mac"
  else if (ua.includes("linux"))
    detectedOS.value = "linux"
})

onMounted(async () => {
  try {
    const res = await axios.get(
      `https://api.github.com/repos/MiyunaDev/${props.appName}/releases`
    )

    releases.value = res.data

    for (const rel of res.data) {
      for (const asset of rel.assets) {
        downloads.value += asset.download_count
      }
    }
  } catch (err) {
    console.error("Failed fetching releases", err)
  } finally {
    loading.value = false
  }
})

const latestRelease = computed(() => releases.value?.[0] || null)

function getIcon(filename: string) {
  const f = filename.toLowerCase()

  // ---------- Android ----------
  if (f.endsWith(".apk")) {
    return {
      icon: PiAndroidLogo,
      label: "Android APK Binary"
    }
  }

  if (f.endsWith(".aab")) {
    return {
      icon: PiAndroidLogo,
      label: "Android App Bundle"
    }
  }

  // ---------- iOS ----------
  if (f.endsWith(".ipa")) {
    return {
      icon: PiDeviceMobileDuotone,
      label: "iOS IPA Package"
    }
  }

  // ---------- Windows ----------
  if (f.endsWith(".exe")) {
    return {
      icon: PiWindowsLogo,
      label: "Windows Installer (.exe)"
    }
  }

  if (f.endsWith(".msi")) {
    return {
      icon: PiWindowsLogo,
      label: "Windows MSI Installer"
    }
  }

  if (f.endsWith(".zip") && f.includes("windows")) {
    return {
      icon: PiWindowsLogo,
      label: "Windows Portable Binary"
    }
  }

  // ---------- Linux ----------
  if (f.endsWith(".deb")) {
    return {
      icon: PiLinuxLogo,
      label: "Ubuntu / Debian Package"
    }
  }

  if (f.endsWith(".rpm")) {
    return {
      icon: PiLinuxLogo,
      label: "Fedora / RHEL RPM Package"
    }
  }

  if (f.endsWith(".pacman")) {
    return {
      icon: PiLinuxLogo,
      label: "Arch Linux Package"
    }
  }

  if (f.endsWith(".snap")) {
    return {
      icon: PiLinuxLogo,
      label: "Snap Binary Package"
    }
  }

  if (f.endsWith(".flatpak")) {
    return {
      icon: PiLinuxLogo,
      label: "Flatpak Package"
    }
  }

  if (f.endsWith(".appimage")) {
    return {
      icon: PiLinuxLogo,
      label: "Portable Linux Binary"
    }
  }

  if (f.endsWith(".tar.gz") || f.endsWith(".tar.xz")) {
    return {
      icon: PiLinuxLogo,
      label: "Linux Binary Archive"
    }
  }

  // ---------- macOS ----------
  if (f.endsWith(".dmg")) {
    return {
      icon: PiMonitorDuotone,
      label: "macOS Disk Image (.dmg)"
    }
  }

  if (f.endsWith(".pkg")) {
    return {
      icon: PiMonitorDuotone,
      label: "macOS Installer Package"
    }
  }

  if (f.endsWith(".app.zip")) {
    return {
      icon: PiMonitorDuotone,
      label: "macOS App Binary"
    }
  }

  // ---------- fallback ----------
  return {
    icon: PiDownloadDuotone,
    label: "Download File"
  }
}

const primaryAsset = computed(() => {
  const assets = latestRelease.value?.assets || []

  if (detectedOS.value === "windows")
    return assets.find((a:any) => a.name.endsWith(".exe"))

  if (detectedOS.value === "android")
    return assets.find((a:any) => a.name.endsWith(".apk"))

  if (detectedOS.value === "ios")
    return assets.find((a:any) => a.name.endsWith(".ipa"))

  if (detectedOS.value === "mac")
    return assets.find((a:any) =>
      a.name.endsWith(".dmg") || a.name.endsWith(".pkg")
    )

  if (detectedOS.value === "linux")
    return assets.find((a:any) =>
      a.name.endsWith(".deb") ||
      a.name.endsWith(".rpm") ||
      a.name.endsWith(".AppImage")
    )

  return assets[0]
})

const otherAssets = computed(() => {
  return (latestRelease.value?.assets || []).filter(
    (a:any) => a.id !== primaryAsset.value?.id
  )
})

</script>

<template>
  <div
    class="group relative overflow-hidden rounded-3xl border border-white/10 bg-gradient-to-br from-slate-900 via-slate-900 to-fuchsia-950 shadow-xl transition hover:-translate-y-1 hover:shadow-fuchsia-500/10"
  >
    <!-- Glow -->
    <div
      class="absolute inset-0 opacity-0 group-hover:opacity-100 transition bg-gradient-to-br from-fuchsia-500/10 to-cyan-500/10"
    ></div>

    <div class="relative p-6 space-y-6">

      <!-- COMING SOON -->
      <div
        v-if="comingSoon"
        class="rounded-2xl border border-yellow-500/20 bg-yellow-500/10 p-4 flex items-start gap-3"
      >
        <PiLockDuotone class="w-5 h-5 mt-0.5 text-yellow-300 shrink-0" />

        <div>
          <p class="font-semibold text-yellow-200">
            {{ releases.length ? "Beta Available" : "Coming Soon" }}
          </p>

          <p class="text-sm text-yellow-100/70">
            {{
              releases.length
                ? "Preview release available. Stability is not guaranteed."
                : "This app flavor has not been released yet."
            }}
          </p>
        </div>
      </div>

      <!-- HEADER -->
      <div class="flex items-center gap-4">
        <img
          :src="appIcon"
          class="w-16 h-16 rounded-2xl object-cover shadow-lg border border-white/10"
        />

        <div class="flex-1 min-w-0">
          <h2 class="text-2xl font-bold text-white truncate">
            {{ appName }}
          </h2>

          <p class="text-sm text-slate-400">
            Miyuna Series Application
          </p>

          <div class="flex flex-wrap gap-3 mt-2 text-xs text-slate-300">
            <span class="flex items-center gap-1">
              <PiArrowCircleDownDuotone class="w-4 h-4" />
              {{ downloads.toLocaleString() }}
            </span>

            <span
              v-if="latestRelease?.tag_name"
              class="flex items-center gap-1"
            >
              <PiTagDuotone class="w-4 h-4" />
              {{ latestRelease.tag_name }}
            </span>
          </div>
        </div>
      </div>

      <!-- PREVIEW -->
      <div
        v-if="!comingSoon && previews.length"
        class="grid grid-cols-2 gap-3"
      >
        <img
          v-for="(img, i) in previews"
          :key="i"
          :src="img"
          @click="zoomedImage = img"
          class="h-32 w-full rounded-2xl object-cover cursor-zoom-in border border-white/10 hover:scale-[1.03] transition"
        />
      </div>

      <!-- FEATURES -->
      <div class="space-y-3 text-sm text-slate-300">
        <div class="flex gap-2">
          <PiPlayDuotone class="w-4 h-4 mt-0.5 text-pink-300" />
          Read & watch in one platform
        </div>

        <div class="flex gap-2">
          <PiBookOpenDuotone class="w-4 h-4 mt-0.5 text-violet-300" />
          EPUB, PDF, iframe/web support
        </div>

        <div class="flex gap-2">
          <PiDeviceMobileDuotone class="w-4 h-4 mt-0.5 text-cyan-300" />
          Mobile & desktop optimized
        </div>

        <div class="flex gap-2">
          <PiGlobeDuotone class="w-4 h-4 mt-0.5 text-emerald-300" />
          Multilingual support
        </div>

        <div class="flex gap-2">
          <PiMonitorDuotone class="w-4 h-4 mt-0.5 text-orange-300" />
          Connected to Honoka ecosystem
        </div>
      </div>

      <!-- WARNING -->
      <div
        v-if="!production"
        class="rounded-2xl border border-red-500/20 bg-red-500/10 p-4 text-sm text-red-200 flex gap-2"
      >
        <PiWarningCircleDuotone class="w-4 h-4 mt-0.5 shrink-0" />
        This application is still in active development.
      </div>

      <!-- PLATFORM -->
      <div v-if="personalPlatforms.length">
        <h4 class="text-sm font-semibold text-slate-300 mb-3 flex gap-2 items-center">
          <PiCpuDuotone class="w-4 h-4" />
          Buildable Platforms
        </h4>

        <div class="flex flex-wrap gap-2">
          <div
            v-for="p in personalPlatforms"
            :key="p.name"
            class="px-3 py-2 rounded-xl bg-white/5 border border-white/10 text-sm flex items-center gap-2"
          >
            <component :is="p.icon" class="w-4 h-4 text-fuchsia-300" />
            {{ p.name }}
          </div>
        </div>
      </div>

      <!-- DOWNLOAD -->
      <div v-if="latestRelease?.assets?.length">
        <h4 class="text-sm font-semibold text-slate-300 mb-3">
          Downloads
        </h4>

        <div v-if="primaryAsset" class="space-y-3">

  <!-- MAIN DOWNLOAD -->
  <a
    :href="primaryAsset.browser_download_url"
    target="_blank"
    class="flex items-center justify-center gap-3 rounded-2xl bg-fuchsia-600 hover:bg-fuchsia-500 px-5 py-4 font-semibold transition"
  >
    <component
      :is="getIcon(primaryAsset.name).icon"
      class="w-5 h-5 shrink-0"
    />

    <span>
      Download {{ getIcon(primaryAsset.name).label }}
    </span>
  </a>

  <!-- TOGGLE -->
  <button
    @click="expanded = !expanded"
    class="w-full flex items-center justify-center gap-2 rounded-2xl border border-white/10 bg-white/5 px-4 py-3 hover:bg-white/10 transition"
  >
    <span>
      {{ expanded ? "Hide Other Downloads" : "Other Downloads" }}
    </span>
  </button>

  <!-- OTHER DOWNLOADS -->
  <div
    v-if="expanded"
    class="grid gap-3"
  >
    <a
      v-for="asset in otherAssets"
      :key="asset.id"
      :href="asset.browser_download_url"
      target="_blank"
      class="flex items-center gap-3 rounded-2xl border border-white/10 bg-white/5 px-4 py-3 hover:bg-white/10 transition"
    >
      <component
        :is="getIcon(asset.name).icon"
        class="w-5 h-5 text-fuchsia-300 shrink-0"
      />

      <div class="min-w-0">
        <p class="text-sm font-medium">
          {{ getIcon(asset.name).label }}
        </p>

        <p class="text-xs text-slate-400 truncate">
          {{ asset.name }}
        </p>
      </div>
    </a>
  </div>

</div>
      </div>

      <!-- LOADING -->
      <div
        v-if="loading"
        class="text-center text-sm text-slate-500"
      >
        Loading release data...
      </div>
    </div>

    <!-- MODAL -->
    <div
      v-if="zoomedImage"
      class="fixed inset-0 z-50 bg-black/80 backdrop-blur-sm flex items-center justify-center p-6"
      @click.self="zoomedImage = null"
    >
      <button
        @click="zoomedImage = null"
        class="absolute top-6 right-6 p-2 rounded-xl bg-white/10 hover:bg-white/20"
      >
        <PiX class="w-5 h-5 text-white" />
      </button>

      <img
        :src="zoomedImage"
        class="max-w-full max-h-full rounded-2xl shadow-2xl"
      />
    </div>
  </div>
</template>