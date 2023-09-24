<!-- eslint-disable prettier/prettier -->
<!-- eslint-disable prettier/prettier -->
<script setup>
import { ref, watch } from 'vue'; // Removed 'onMounted' import
import { useLayout } from '@/layout/composables/layout';

import OrderComponent from '@/components/OrderComponent.vue';
import IncomeComponent from '@/components/IncomeComponent.vue';
import RecentSalesComponent from '@/components/RecentSales.vue';
import LineChartComponent from '@/components/LineChartComponent.vue';
import NotificationComponent from '../components/NotificationComponent.vue';

const { isDarkTheme } = useLayout();

const lineOptions = ref(null);

const applyLightTheme = () => {
    lineOptions.value = {
        plugins: {
            legend: {
                labels: {
                    color: '#495057'
                }
            }
        },
        scales: {
            x: {
                ticks: {
                    color: '#495057'
                },
                grid: {
                    color: '#ebedef'
                }
            },
            y: {
                ticks: {
                    color: '#495057'
                },
                grid: {
                    color: '#ebedef'
                }
            }
        }
    };
};

const applyDarkTheme = () => {
    lineOptions.value = {
        plugins: {
            legend: {
                labels: {
                    color: '#ebedef'
                }
            }
        },
        scales: {
            x: {
                ticks: {
                    color: '#ebedef'
                },
                grid: {
                    color: 'rgba(160, 167, 181, .3)'
                }
            },
            y: {
                ticks: {
                    color: '#ebedef'
                },
                grid: {
                    color: 'rgba(160, 167, 181, .3)'
                }
            }
        }
    };
};

watch(
    isDarkTheme,
    (val) => {
        if (val) {
            applyLightTheme();
        } else {
            applyDarkTheme();
        }
    },
    { immediate: true }
);
</script>

<template>
    <div class="container">
        <div class="row">
            <div class="col-7">
                <div class="same-width">
                    <OrderComponent />
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-12">
                <div class="same-width">
                    <div class="side-by-side">
                        <RecentSalesComponent />
                        <IncomeComponent />
                    </div>
                </div>
                <div class="same-width">
                    <LineChartComponent />
                </div>
            </div>
            <div class="col-12">
                <div class="same-width">
                    <NotificationComponent />
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.container {
    display: flex;
    flex-direction: column;
    gap: 20px; /* Adjust the gap as needed */
}

.row {
    display: flex;
    flex-direction: row;
    gap: 20px;
}

.col-7,
.col-5,
.col-12 {
    flex: 1;
}

.side-by-side {
    display: flex;
    flex-direction: row;
    gap: 20px;
}

.same-width {
    flex: 1;
}
</style>
