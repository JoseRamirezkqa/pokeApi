<template>
    <div>
        <error v-if="this.error"/>
        <loading v-if="this.isLoading && !this.error"/>
        <div v-if="!this.isLoading && !this.error" class=" dark:text-[#94088d]">
            <h1 class="text-center mt-[1.25rem] font-bold text-[2.5rem]">
                {{ name }}
            </h1>
            <div class="md:flex w-[90%] md:h-[15.625rem] m-auto mt-[1.25rem] ">
                <img :src="urlImage" class="md:basis-1/12 md:h-[100%] :w-[250px] h-[250px] m-auto">
                <div class="md:basis-11/12	 border-2 border-black dark:border-[#94088d] border-dashed rounded-[1.25rem] h-[100%]">
                    <div class="grid grid-cols-2">
                        <div class="pl-[0.625rem]">
                            <h1 class="font-bold text-[2.5rem] dark:text-[#94088d]">
                                Stats
                            </h1>
                            <h1 v-for="i, index in stats" class="flex  font-bold text-[1.25rem] md:ml-[1.25rem]">{{ nameStats[index]}}:
                                <p class="font-normal">{{ base[index] }}</p>
                            </h1>
                        </div>
                        <div>
                            <h1 class="font-bold text-[2.5rem] dark:text-[#94088d]">
                                Abilities
                            </h1>
                            <ol class="list-disc">
                                <li v-for="i, index in abilities" class="text-[1.25rem] ml-[2.5rem]">
                                    {{ ability[index] }}
                                </li>
                            </ol>
                        </div>
                    </div>
                </div>
            </div>
            <div class="grid grid-cols-2 md:grid-cols-3 w-[90%] md:w-[60%] place-items-center m-auto mt-[1.25rem]">
                <h1 class="font-bols col-span-2 md:col-span-3 font-bold text-[2.5rem]">Pictures</h1>
                <img :src="images[index]" v-for="i, index in images" class="md:w-[50%]">
            </div>
        </div>

    </div>
</template>
<script>
export default {
    name: 'PagePokemon',
    data() {
        return {
            name: String,
            urlImage: String,
            url: 'https://pokeapi.co/api/v2/pokemon',
            nameStats: [],
            base: [],
            stats: [],
            ability: [],
            abilities: [],
            images: [],
            isLoading: false,
            error : false,
        }
    },
    methods: {
        async create() {
            try {
                this.isLoading = true;
                const response = await this.$axios.get(this.url + `/${this.name}`);
                this.name = response.data.name
                let { front_default } = response.data.sprites.other.home
                this.urlImage = front_default
                let { stats } = response.data
                this.stats = stats
                this.stats.forEach(el => {
                    this.nameStats.push(el.stat.name)
                    this.base.push(el.base_stat)
                })
                this.abilities = response.data.abilities
                this.abilities.forEach(el => {
                    this.ability.push(el.ability.name)
                })
                const { sprites } = response.data
                let sprite = Object.values(sprites)
                for (let i = 0; i < sprite.length - 2; i++) {
                    if (sprite[i] != null) {
                        this.images.push(sprite[i])
                    }
                }
            }
            catch{
                this.error = true
            }
            finally {
                this.isLoading = false
            }
        }
    },
    mounted() {
        this.name = this.$route.params.name
        this.create()
    }
}
</script>