<template>
  <div>
    <loading v-if="this.isLoading"/>
    <div class="w-full " v-else="this.isLoading">
      <div class="2xl:col-span-2	justify-self-end	2xl:mr-[3.125rem] my-[0.313rem]">
        <div class="flex border  duration-300 rounded-full 2xl:w-[15.625rem] 2xl:h-[2.5rem] justify-center 
                     items-center focus-within:border-black w-[15rem] h-[3.125rem] dark:hover:border-[#94088d] dark:focus-within:border-[#94088d] dark:border-black">
          <img src="icons8-búsqueda-50.png" class="2xl:w-[1.875rem] 2xl:h-[1.875rem] 2xl:mr-[0.188rem] w-[1.8rem]"
            alt="No sirve esta mierda">
          <input type="text" v-model="busquedaPalabra" placeholder="shearch"
            class="2xl:h-[1.875rem] 2xl:w-[12.5rem] w-[10rem] bg-green-200/10 focus:outline-0 dark:bg-black/25 dark:text-[#94088d] dark:placeholder:text-[#94088d]" @keydown.enter="busqueda">
        </div>
      </div>
      <div class=" pt-[1.25rem]">
        <div class="grid 2xl:grid-cols-4 min-[640px]:grid-cols-2 lg:grid-cols-3 justify-items-center gap-y-12">
          <card v-for="i, index in names" :url="img[index]" :name="names[index]" :key="index"  @otherPage="otherPage(index)" @newPage="newPage(index)"/>
        </div>
        <div class="text-center mt-[2.5rem] ">
          <button class="border border-black rounded-full w-[6.25rem] h-[3.125rem] 
                              hover:bg-green-400 duration-300 hover:scale-125 mr-8 dark:hover:bg-[#121212] dark:text-[#94088d] dark:border-[#94088d]" @click="PreviousPage"
            v-if="banderaPage">
            Previous Page
          </button>
          <button class="border border-black rounded-full w-[6.25rem] h-[3.125rem] 
                              hover:bg-green-400 duration-300 hover:scale-125 dark:hover:bg-[#121212] dark:text-[#94088d] dark:border-[#94088d]" @click="NextPage">
            Next Page
          </button>
        </div>
      </div>
    </div>

  </div>
</template>
<script>
export default {
  name: 'IndexPage',
  layout: 'error',
  data() {
    return {
      image: true,
      url: 'https://pokeapi.co/api/v2/pokemon',
      names: [],
      img: [],
      inicio: 1,
      final: 12,
      finalApi: Number,
      banderaPage: true,
      busquedaPalabra: '',
      isLoading: false,
    }
  },
  methods: {
    async create() {
      try {
        this.isLoading = true;
        for (let i = this.inicio; i <= this.final; i++) {
          const response = await this.$axios.get(this.url + '/' + i)
          let { name } = response.data
          this.names.push(name)
          let { front_default } = response.data.sprites.other.home;
          this.img.push(front_default)
        }
      }
      finally {
        this.isLoading = false
        localStorage.inicio = this.inicio
        localStorage.final = this.final
      }
    },
    otherPage(index ){
      window.open(`/${this.names[index]}`, '_blank');
    },  
    NextPage() {
      this.names = []
      this.img = []
      this.inicio = this.final + 1
      this.final += 12
      this.buttons()
      this.create()
    },
    PreviousPage() {
      this.names = []
      this.img = []
      this.inicio -=  12
      this.final -= 12
      this.buttons()
      this.create()
    },
    newPage(index) {
      this.$router.push(`/${this.names[index]}`)
    },
   busqueda() {
      this.$router.push(`/${this.busquedaPalabra}`)
    },
    buttons() {
      this.banderaPage = this.inicio == 1 ? false : true;

    },
  },
  mounted() {
    if (localStorage.getItem('inicio') != undefined && localStorage.getItem('final') != undefined) {
      this.inicio = parseInt(localStorage.getItem('inicio'))
      this.final = parseInt(localStorage.getItem('final'))

    } else {
      this.inicio = 1
      this.final = 12
    } 
    this.buttons()
    this.create()
  },

}
</script>
