<script setup>
import { ref } from 'vue';

const items = ref([
    { label: 'Add New', icon: 'pi pi-fw pi-plus' },
    { label: 'Remove', icon: 'pi pi-fw pi-minus' }
]);
</script>

<template>
    <div class="col-12 xl:col-6">
        <div class="card">
            <div class="flex align-items-center justify-content-between mb-4">
                <h5>Notifications</h5>
                <div>
                    <Button icon="pi pi-ellipsis-v" class="p-button-text p-button-plain p-button-rounded" @click="$refs.menu1.toggle($event)"></Button>
                    <Menu ref="menu1" :popup="true" :model="items"></Menu>
                </div>
            </div>

            <span class="block text-600 font-medium mb-3">TODAY</span>
            <ul class="p-0 mx-0 mt-0 mb-4 list-none">
                <li class="flex align-items-center py-2 border-bottom-1 surface-border">
                    <div class="w-3rem h-3rem flex align-items-center justify-content-center bg-blue-100 border-circle mr-3 flex-shrink-0">
                        <i class="pi pi-dollar text-xl text-blue-500"></i>
                    </div>
                    <span class="text-900 line-height-3"
                        >fero@boom.mn
                        <span class="text-700">Хийгдээгүй <span class="text-blue-500">79$</span></span>
                    </span>
                </li>
                <li class="flex align-items-center py-2">
                    <div class="w-3rem h-3rem flex align-items-center justify-content-center bg-orange-100 border-circle mr-3 flex-shrink-0">
                        <i class="pi pi-download text-xl text-orange-500"></i>
                    </div>
                    <span class="text-700 line-height-3">ireed avsan <span class="text-blue-500 font-medium">2500$</span> хийсэн</span>
                </li>
            </ul>

            <span class="block text-600 font-medium mb-3">YESTERDAY</span>
            <ul class="p-0 m-0 list-none">
                <li class="flex align-items-center py-2 border-bottom-1 surface-border">
                    <div class="w-3rem h-3rem flex align-items-center justify-content-center bg-blue-100 border-circle mr-3 flex-shrink-0">
                        <i class="pi pi-dollar text-xl text-blue-500"></i>
                    </div>
                    <span class="text-900 line-height-3"
                        >fero@boom.mn
                        <span class="text-700">ireed avsan<span class="text-blue-500">59$</span></span>
                    </span>
                </li>
                <li class="flex align-items-center py-2 border-bottom-1 surface-border">
                    <div class="w-3rem h-3rem flex align-items-center justify-content-center bg-pink-100 border-circle mr-3 flex-shrink-0">
                        <i class="pi pi-question text-xl text-pink-500"></i>
                    </div>
                    <span class="text-900 line-height-3"
                        >fero@boom.mn
                        <span class="text-700">маргааш ирж авна</span>
                    </span>
                </li>
            </ul>
        </div>
    </div>
</template>

<style scoped>
/* Add CSS for notifications */
.notifications-container {
    position: absolute;
    top: 0;
    right: 0; /* Adjust the width as needed */
    background-color: rgba(0, 0, 0, 0.8); /* Background color for notifications */
    color: #fff; /* Text color for notifications */
    padding: 16px;
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    overflow-y: auto; /* Add scrollbars if needed */
    max-height: 80vh; /* Limit the maximum height of notifications */
}

.notification {
    margin-bottom: 8px;
}
</style>
