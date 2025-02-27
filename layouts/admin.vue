<template>
    <div>
        <Header />
    </div>
    <div class="p-5">
        <slot />
    </div>
</template>
<script setup>
import Header from "@/navigations/header.vue";
</script>