<template>
    <div class="w-[18.75rem] h-[18.75rem] shadow-2xl b-red  text-center text-[30px] bg-green-300/50 cursor-pointer hover:bg-green-300  hover:scale-125 duration-500 
    dark:bg-[#212021]/75 dark:hover:bg-[#121212] dark:text-[#94088d] dark:font-bold dark:text-[40px]" 
    @click="newPage" @auxclick="otherPage">
        <img :src="url" alt="" class="w-full h-[80%]">
        <h1>
            {{ name }}
        </h1>
    </div>
</template>
<script>
export default{
    name: 'card',
    props:{
        url: String,
        name: String,
    },
    methods:{
        newPage(){
            this.$emit('newPage');
        },
        otherPage(){
            this.$emit('otherPage');
        }
    }
}
</script>