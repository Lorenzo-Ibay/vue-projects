<template>
    <div>
        <Suspense>
            <AsyncCityView />
            <!-- <CityViewSkeleton /> -->
            <template #fallback>
                <CityViewSkeleton />
            </template>
        </Suspense>
    </div>
</template>

<script setup>
import CityViewSkeleton from '../components/CityViewSkeleton.vue';
import AsyncCityView from './AsyncCityView.vue';


</script>
