<template>
    <div class="flex flex-col flex-1 items-center ">
        <!-- Banner -->
        <div v-if="route.query.preview" class="text-white p-4 bg-weather-secondary w-full text-center ">
            <p>
                You are Currently Viewing the city click on the "+" icon to start tracking the city
            </p>
        </div>
        <!-- OverView -->
        <div class="flex flex-col items-center text-white py-12">
            <h1 class="text-4xl mb-2 "> {{ route.params.city }}</h1>
            <p class="text-sm mb-12 ">
                {{
                new Date(weatherReport.current.dt * 1000).toLocaleString("en-us",
                    {
                        weekday: "short",
                        day: "2-digit",
                        month: "long",
                    }
                 )
                }}
                {{
                    new Date(weatherReport.current.dt * 1000).toLocaleString("en-us",
                    {
                        timeStyle: "short",
                    }
                    )
                }}
            </p>
            <p class="text-8xl mb-8">
                {{ Math.round(weatherReport.current.temp) }} &deg;
            </p>
                <p>
                    Feels Like
                    {{ Math.round(weatherReport.current.feels_like) }} &deg;
                </p>
                <p class="capitalize">{{ weatherReport.current.weather[0].description }}</p>
                <img class="w-[150px] h-auto " :src="`https://openweathermap.org/img/wn/${weatherReport.current.weather[0].icon}@2x.png`" alt="">
        </div>
        <hr class="border-white border-opacity-10 border w-full " />
        <!-- hourly -->
        <div class="max-w-screen-md w-full py-12">
            <div class="mx-8 text-white">
                <h2 class="mb-4">Hourly Forecast</h2>
                <div class="flex gap-10 overflow-x-scroll">
                    <div class="flex flex-col gap-4 items-center"  v-for="hourData in weatherReport.hourly" :key="hourData.dt">
                        <p class=" whitespace-nowrap text-md ">
                            {{ new Date(hourData.dt * 1000).toLocaleString("en-us", { hour: "numeric",}) }}
                        </p>
                        <img class="w-auto h-[50px] object-cover" :src="`https://openweathermap.org/img/wn/${hourData.weather[0].icon}@2x.png`" alt="">
                        <p class="text-xl">
                            {{ Math.round(hourData.temp) }} &deg;
                        </p>
                    </div>
                </div>
            </div>
        </div>
        <hr class="border-white border-opacity-10 border w-full " />
        <!-- weekly -->
        <!-- <div class="max-w-screen-md w-full py-12">

        </div> -->
    </div>
</template>

<script setup>
import axios from 'axios';
import { useRoute } from 'vue-router';


const route = useRoute();
const getWeatherData = async () => {
    try {
        // const weatherData = await axios.get(`http://api.openweathermap.org/data/2.5/weather?q=${route.params.city},${route.params.state}&APPID=8de8f30d2be4c0ea624240f17768165a&units=metricnp`);
        const weatherData = await axios.get(`https://api.openweathermap.org/data/3.0/onecall?lat=${route.query.lat}&lon=${route.query.lng}&APPID=8de8f30d2be4c0ea624240f17768165a&units=metric`);
        // date and time offset (timezones) for current weather report
        const localOffset = new Date().getTimezoneOffset() * 60000;
        const utc = weatherData.data.dt * 1000 + localOffset;
        weatherData.data.currentTime = utc + 1000 * weatherData.timezone_offset;
        // hourly weather reports with timezone offset
        weatherData.data.hourly.forEach(function (hour){
            const utc = hour.dt * 1000 * localOffset;
            hour.currentTime = utc * 1000 + weatherData.timezone_offset;
        });
        // console.log(weatherData);
        return weatherData.data;
    } catch (error) {
        console.log(error);
    }
};
const weatherReport = await getWeatherData();

</script>