<template>
    <div>
        <UCard>
            <template #header>
                <h1 class="font-bold">Nuxt 3</h1>
            </template>
            <div class="block space-y-4">
                Welcome To John Nuxt Starter Template
            </div>
            <div class="flex items-center justify-center mt-4">
               
            </div>
            <template #footer>
             <UButton>This is A Button</UButton>
            </template>
        </UCard>
    </div>
</template>
<script setup>
import {ref,onMounted} from "vue";
import axios from "axios";

definePageMeta({
  layout: 'admin'
})

//sample function format
// const sampleFunction = () => {

// }

onMounted(() => {
//   onload a function if needed wxample sampleFunction();
});
</script>