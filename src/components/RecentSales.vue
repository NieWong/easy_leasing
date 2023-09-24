<script setup>
import { onMounted, ref } from 'vue';
import ProductService from '../service/ProductService';

const products = ref(null);
const productService = new ProductService();

const formatCurrency = (value) => {
    return value.toLocaleString('en-US', { style: 'currency', currency: 'USD' });
};

onMounted(() => {
    productService.getProductsSmall().then((data) => (products.value = data));
});
</script>

<template>
    <div class="col-12 lg:col-6 xl:col-4" style="width: 550px">
        <div class="card">
            <h5>Recent Sales</h5>
            <DataTable :value="products" :rows="5" :paginator="true" responsiveLayout="scroll">
                <Column style="width: 15%">
                    <template #header> Image </template>
                    <template #body="slotProps">
                        <img :src="'demo/images/product/' + slotProps.data.image" :alt="slotProps.data.image" width="50" class="shadow-2" />
                    </template>
                </Column>
                <Column field="name" header="Name" :sortable="true" style="width: 35%"></Column>
                <Column field="price" header="Price" :sortable="true" style="width: 35%">
                    <template #body="slotProps">
                        {{ formatCurrency(slotProps.data.price) }}
                    </template>
                </Column>
                <Column style="width: 15%">
                    <template #header> View </template>
                    <template #body>
                        <Button icon="pi pi-search" type="button" class="p-button-text"></Button>
                    </template>
                </Column>
            </DataTable>
        </div>
    </div>
</template>
