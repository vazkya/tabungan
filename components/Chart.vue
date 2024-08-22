<script setup lang="ts">
import Chart from 'chart.js/auto';

const labels = [
    'Januari',
    'Februari',
    'March',
    'April',
    'May',
    'June',
];

    const data = {
        labels: labels,
        datasets: [{
            label: 'My First dataset',
            backgroundColor: 'rgb(211, 211,211)',
            borderColor: 'rgb(2555, 99, 132)',
            data: [5 , 10, 5, 2, 20, 30, 45],
        }]
    };

    const config = {
        type: 'bar',
        data: data,
        Options: {}
    };

onMounted(() => {
    const myChart = new Chart (
        document.getElementById('myChart'),
        config
    );
})
</script>

<template>
    <div>
        <canvas id="myChart"></canvas>
    </div>
</template>