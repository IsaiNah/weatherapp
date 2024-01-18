<template>
    <div class="flex py-6 bg-weather-secondary rounded-md shadow-md
    cursor-pointer">
         <div class="flex flex-col flex-1">
             <h1 class="text-3xl">{{ city.city }}</h1>
             <h3> {{ city.state }}</h3>
         </div>
         <div class="flex flex-col gap-2">
            <p class="text-3xl self-end">
                {{ city.temp }}&deg;
            </p>
         </div>
    </div>
</template>

<script setup>
defineProps(
    {
       
        city:{
            type: Object,
            default: () => {},
        },
    });
</script>