<template>
    <div class="flex flex-col flex-1 items-center ">
        <!-- Banner -->
        <div v-if="route.query.preview" class="text-white p-4 bg-weather-secondary w-full text-center ">
            <p>
                You are Currently Viewing the city click on the "+" icon to start tracking the city
            </p>
        </div>
        <!-- OverView -->
        <div class="">

        </div>
    </div>
</template>

<script setup>
import axios from 'axios';
import { useRoute } from 'vue-router';


const route = useRoute();
const getWeatherData = async () => {
    try {
        const weatherData = await axios.get(`http://api.openweathermap.org/data/2.5/weather?q=${route.params.city},${route.params.state}&APPID=8de8f30d2be4c0ea624240f17768165a&units=metric`);
        // // const weatherData = await axios.get(`https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&APPID=8de8f30d2be4c0ea624240f17768165a`);
        // date and time offset (timezones) for current weather report
        const localOffset = new Date().getTimezoneOffset() * 60000;
        const utc = weatherData.data.dt * 1000 + localOffset;
        weatherData.data.currentTime = utc + 1000 * weatherData.timezone_offset;
        // hourly weather reports with timezone offset
        // weatherData.data.hourly.forEach(function (hour){
        //     const utc = hour.dt * 1000 * localOffset;
        //     hour.currentTime = utc * 1000 + weatherData.timezone_offset;
        // });
        console.log(weatherData);
        return;
    } catch (error) {
        console.log(error);
    }
};
const weatherReport = await getWeatherData();
console.log(weatherReport);
</script>