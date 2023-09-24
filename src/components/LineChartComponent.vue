<script setup>
import { reactive } from 'vue';
const lineData = reactive({
    labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
    datasets: [
        {
            label: '1 data',
            data: [65, 59, 80, 81, 56, 55, 40],
            fill: false,
            backgroundColor: '#2f4860',
            borderColor: '#2f4860',
            tension: 0.4
        },
        {
            label: '2 data',
            data: [28, 48, 40, 19, 86, 27, 90],
            fill: false,
            backgroundColor: '#00bb7e',
            borderColor: '#00bb7e',
            tension: 0.4
        }
    ]
});
</script>

<template>
    <div class="col-12 lg:col-6 xl:col-11" style="width: 650px;">
        <div class="card">
            <h5>Sales Overview</h5>
            <Chart type="line" :data="lineData" :options="lineOptions" />
        </div>
    </div>
</template>
