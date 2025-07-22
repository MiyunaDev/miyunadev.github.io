<script lang="ts" setup>
import axios from "axios"
import { ref, onMounted } from "vue"

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
    PiLockDuotone
} from "vue-icons-plus/pi"

const props = defineProps<{
    appName: string
    previews: string[]
    production: boolean,
    personalPlatforms: { name: string; icon: any }[]
    comingSoon?: boolean
}>()

const releases = ref<any[]>([])
const downloads = ref<number>(0)
const zoomedImage = ref<string | null>(null)

onMounted(async () => {
    try {
        const res = await axios.get(`https://api.github.com/repos/MiyunaDev/${props.appName}/releases`)
        releases.value = res.data
        for (const r of res.data) {
            for (const asset of r.assets) {
                downloads.value += asset.download_count
            }
        }
    } catch (e) {
        console.error("Failed fetching releases", e)
    }
})

function getIcon(filename: string) {
    const f = filename.toLowerCase()
    if (f.endsWith(".apk") || f.endsWith(".aab"))
        return { icon: PiAndroidLogo, label: f.endsWith(".aab") ? "Android Bundle" : "Android APK" }
    if (f.endsWith(".exe")) return { icon: PiWindowsLogo, label: "Windows EXE" }
    if ([".deb", ".rpm", ".pacman", ".snap", ".appimage"].some(ext => f.endsWith(ext)))
        return { icon: PiLinuxLogo, label: f.split(".").pop()?.toUpperCase() }
    return { icon: PiDownloadDuotone, label: "Download" }
}
</script>

<template>
    <div class="bg-gradient-to-br from-[#1f001c] via-[#2c0026] to-[#1a001a] rounded-2xl p-6 shadow-lg space-y-8 w-full">

        <!-- Coming Soon Overlay -->
        <div v-if="props.comingSoon" class="relative rounded-xl overflow-hidden border border-yellow-500/20">
            <div
                class="absolute inset-0 bg-gradient-to-br from-yellow-900/70 via-yellow-800/40 to-yellow-900/70 backdrop-blur-sm z-10 flex flex-col items-center justify-center text-yellow-200 px-6 py-10 space-y-2">
                <PiLockDuotone class="animate-pulse text-3xl" />
                <p class="text-lg font-semibold tracking-wide">Coming Soon</p>
                <p class="text-sm text-yellow-100/80 text-center">
                    This flavor is not yet available. Stay tuned for the next release!
                </p>
            </div>
            <img :src="previews[0]" class="w-full h-40 object-cover opacity-20 blur-sm" alt="Coming Soon Preview" />
        </div>


        <!-- Header -->
        <div class="text-center space-y-2">
            <h2 class="text-3xl font-bold text-pink-300">{{ appName }}</h2>
            <p class="text-sm text-gray-400 italic">A Miyuna Series Application</p>
            <div class="flex justify-center items-center gap-4 text-sm text-white/80">
                <span class="flex items-center gap-1">
                    <PiArrowCircleDownDuotone class="w-5 h-5" /> {{ downloads.toLocaleString() }} downloads
                </span>
                <span v-if="releases[0]?.tag_name" class="flex items-center gap-1">
                    <PiTagDuotone class="w-5 h-5" /> Version {{ releases[0].tag_name }}
                </span>
            </div>
        </div>

        <!-- Preview Gallery -->
        <div v-if="!comingSoon" class="grid grid-cols-2 md:grid-cols-3 gap-4">
            <img v-for="(src, i) in previews" :key="i" :src="src" @click="zoomedImage = src"
                class="object-cover w-full h-40 rounded-xl shadow-md cursor-zoom-in hover:scale-[1.03] transition"
                alt="Preview" />
        </div>

        <!-- Zoom Modal -->
        <div v-if="zoomedImage" class="fixed inset-0 z-50 bg-black/80 flex items-center justify-center"
            @click.self="zoomedImage = null">
            <img :src="zoomedImage" class="max-w-full max-h-full rounded-lg shadow-2xl" />
            <button @click="zoomedImage = null" class="absolute top-4 right-4 text-white text-2xl">✕</button>
        </div>

        <!-- Feature Overview -->
        <div class="space-y-2 text-white/90 text-sm">
            <div class="flex items-center gap-2">
                <PiPlayDuotone class="w-5 h-5 text-pink-300" />
                Read & Watch content in one unified platform
            </div>
            <div class="flex items-center gap-2">
                <PiBookOpenDuotone class="w-5 h-5 text-purple-300" />
                Epub, PDF, Iframe (Web) support and more
            </div>
            <div class="flex items-center gap-2">
                <PiDeviceMobileDuotone class="w-5 h-5 text-blue-300" />
                Optimized for Desktop & Mobile
            </div>
            <div class="flex items-center gap-2">
                <PiGlobeDuotone class="w-5 h-5 text-teal-300" />
                Multilingual Support
            </div>
            <div class="flex items-center gap-2">
                <PiMonitorDuotone class="w-5 h-5 text-rose-300" />
                Connected to the Miyuna Series Ecosystem (Honoka and more)
            </div>
        </div>

        <!-- Development Warning -->
        <div v-if="!production"
            class="bg-red-900/30 border border-red-600 text-red-300 px-4 py-3 rounded-lg flex items-center gap-3">
            <PiWarningCircleDuotone class="hidden md:block w-5 h-5 text-red-400" />
            This application is currently in development and may be unstable or incomplete.
        </div>

        <!-- Download Buttons -->
        <div v-if="releases[0]?.assets.length" class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <a v-for="asset in releases[0].assets" :key="asset.id" :href="asset.browser_download_url" target="_blank"
                class="flex items-center gap-4 px-4 py-3 rounded-xl bg-gradient-to-r from-[#351d38] via-[#50284f] to-[#351d38] 
              hover:brightness-110 transition text-white shadow-inner border border-white/10 backdrop-blur-md">
                <component :is="getIcon(asset.name).icon" class="w-6 h-6 shrink-0 text-pink-200" />
                <div class="flex flex-col">
                    <span class="font-semibold text-sm">{{ getIcon(asset.name).label }}</span>
                    <span class="text-xs opacity-70 truncate">{{ asset.name }}</span>
                </div>
            </a>
        </div>

        <div class="mt-6">
            <h4 class="text-pink-300 font-semibold mb-2 flex items-center gap-2">
                <PiGearSixDuotone class="w-5 h-5" />
                Buildable Platforms
            </h4>
            <div class="flex flex-wrap gap-3 text-white text-sm">
                <div v-for="p in personalPlatforms" :key="p.name"
                    class="flex items-center gap-2 bg-white/10 rounded px-3 py-1">
                    <component :is="p.icon" class="w-5 h-5 text-pink-400" />
                    <span>{{ p.name }}</span>
                </div>
            </div>
        </div>
    </div>
</template>