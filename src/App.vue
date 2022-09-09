<script setup>
import {ref} from "vue"
import { nanoid } from "nanoid"
import { useStorage } from "@vueuse/core" 
import  confetti   from "canvas-confetti"

const newGrocery = ref("")
const groceries = useStorage("groceries", [])

const addGrocery = () => {
    if(newGrocery.value) {
        groceries.value.push({id: nanoid(), name: newGrocery.value })
        newGrocery.value = " "


    }
}

const deleteGrocery = id => {
    const removeIndex = groceries.value.findIndex(grocery => grocery.id === id)
    groceries.value.splice(removeIndex, 1)
    confetti({ particleCount: 1000, spread: 1000, origin: { y: 1 } })


}

</script>

<template>
    <main class="">
 <h1 class="title"> 🥝   Vue Grocery List 🥕</h1>
 
 <form class="newGroceryForm" @submit.prevent="addGrocery">
        <input id="newGrocery"  autocomplete="false" type="text" placeholder="Add an item to your list."
        v-model="newGrocery"
        />
    
        <button type="submit" >Insert</button>
    </form>
    
    <ul>
        <li v-for="grocery in groceries" @click="deleteGrocery(grocery.id)">{{ grocery.name }}</li>
    </ul>
    <h3>Pending Items: {{groceries.length}}</h3>
    </main>
    
</template>

<style lang="postcss" scoped>

main {
    @apply mt-8 flex flex-col justify-center items-center gap-8;

.title {
    @apply m-2 text-6xl  tracking-wider font-bold text-black;
}
form {
    @apply flex focus-within:ring-8 focus-within:ring-black  ;
    input {
        @apply bg-white font-bold text-black p-2 w-80 text-2xl rounded-l-md outline-none; 
    }
    button {
        @apply text-background p-2  text-2xl font-bold rounded-r-md outline-none ;
    
    &:hover {
        @apply bg-purple-300;
    }
}

}
 ul {
    @apply flex flex-col items-center font-bold justify-center rounded-lg bg-black;
     li {
        @apply bg-white text-black m-2 p-2 w-96 text-center;
        &:hover {
            @apply bg-purple-300 font-bold cursor-pointer;
        }
    }
 }

}
</style>
