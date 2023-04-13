<template>
    <div class="bg-green-200 min-h-screen  font-[custom] bg-image dark:bg-black">
        <div class="w-full h-[10%] bg-green-300/75 rounded-b-[15px] text-center dark:bg-black dark:text-[#94088d]">
            <nuxt-link class="text-[40px] cursor-pointer hover:text-[60px] duration-1000" to="/">
                PokeDEx
            </nuxt-link>
            <button @click="darkMode" class="border rounded-full border-black dark:border-[#94088d] h-[30px] min-[640px]:h-[50px] 
                                            absolute right-5 mt-[10px] min-[640px]:mt-[5px] w-[50px] min-[640px]:w-[100px]">
                <i class="fa-solid fa-sun" v-if="this.banderaIcon"></i>
                <i class="fa-solid fa-moon" v-if="!this.banderaIcon"></i>
                <p class="max-[640px]:hidden inline-block">{{theme}} mode</p>
            </button>
        </div>
        <Nuxt class="" />
    </div>
</template>
<script>
export default {
    name: 'LayoutDefault',
    data() {
        return {
            theme: '',  
            themeBoolean: Boolean,
            banderaIcon: Boolean
        }
    },
    methods: {
        darkMode() {
            this.banderaIcon = !this.banderaIcon
            this.themeBoolean = document.documentElement.classList.toggle('dark')
            this.themeBoolean ? localStorage.theme = 'dark' : localStorage.theme = 'ligth'
            this.theme = localStorage.theme
        }
    },
    mounted() {
        if (localStorage.theme === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
            document.documentElement.classList.add('dark')
            this.theme = 'dark'
            this.banderaIcon = false
        } else {
            document.documentElement.classList.remove('dark')
            this.theme = 'light'
            this.banderaIcon = true
        }
    }
}
</script>