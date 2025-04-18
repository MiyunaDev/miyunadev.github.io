<script lang="ts" setup>
import axios from "axios"
import { ref, onMounted } from "vue"
import {
    FaAndroid,
    FaWindows,
    FaLinux,
    FaDownload,
} from "vue-icons-plus/fa"

const props = defineProps({
    appName: String,
    preview: String,
    production: Boolean
})

const releases = ref<any[]>([])

onMounted(() => {
    axios
        .get(`https://api.github.com/repos/MiyunaDev/${props.appName}/releases`)
        .then((res) => {
            releases.value = res.data
            console.log(res.data)
        })
})

function getIcon(filename: string) {
    const lower = filename.toLowerCase()

    if (lower.endsWith(".apk")) return { icon: FaAndroid, label: "Android (.apk)" }
    if (lower.endsWith(".aab")) return { icon: FaAndroid, label: "Android Bundle (.aab)" }
    if (lower.endsWith(".exe")) return { icon: FaWindows, label: "Windows (.exe)" }
    if (lower.endsWith(".deb")) return { icon: FaLinux, label: "Debian/Ubuntu (.deb)" }
    if (lower.endsWith(".rpm")) return { icon: FaLinux, label: "Fedora (.rpm)" }
    if (lower.endsWith(".pacman")) return { icon: FaLinux, label: "Arch Linux (.pacman)" }
    if (lower.endsWith(".snap")) return { icon: FaLinux, label: "Snap (.snap)" }
    if (lower.endsWith(".appimage")) return { icon: FaLinux, label: "AppImage" }

    return { icon: FaDownload, label: "Download" }
}

</script>

<template>
    <div class="flex flex-col items-center gap-6 p-6">
        <p class="text-2xl font-semibold text-center">{{ appName }}</p>

        <div class="flex flex-col md:flex-row items-center gap-6">
            <img :src="preview" class="w-full md:w-1/2 rounded shadow" />

            <div v-if="releases.length">
                <p class="text-lg font-medium mb-2">
                    Latest Version: {{ releases[0].tag_name }}
                </p>
                <p v-if="!production" class="text-yellow-600 bg-[#450a53] p-2 rounded border border-red-300 my-4">
                    ⚠️ This application is currently in development and not fully released.
                </p>
                <div class="flex flex-wrap gap-4">
                    <a v-for="asset in releases[0].assets" :key="asset.id" :href="asset.browser_download_url"
                        target="_blank" rel="noopener noreferrer"
                        class="flex items-center gap-2 px-4 py-2 bg-[#380046] hover:bg-[#39213f] text-white rounded shadow transition">
                        <component :is="getIcon(asset.name).icon" class="text-lg" />
                        {{ getIcon(asset.name).label }}
                    </a>
                </div>
            </div>
        </div>
    </div>
</template>