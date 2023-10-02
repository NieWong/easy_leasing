<!-- eslint-disable prettier/prettier -->
<script setup>
import { FilterMatchMode } from 'primevue/api';
import { ref, onMounted, onBeforeMount } from 'vue';
import ProductService from '@/service/ProductService';
import { useToast } from 'primevue/usetoast';
const calendarValue = ref(null);

const toast = useToast();

const products = ref(null);
const productDialog = ref(false);
const deleteProductDialog = ref(false);
const deleteProductsDialog = ref(false);
const product = ref({
    first_name: '',
    last_name: '',
    email: '',
    registration: '',
    dateOfBirth: null,
    loan_amount: '',
    category: '', 
    inventoryStatus: null,
    credit_score: '',
    description: ''
});
const selectedProducts = ref(null);
const dt = ref(null);
const filters = ref({});
const submitted = ref(false);
const statuses = ref([
    { label: '1 month', value: 'one_month' },
    { label: '6 month', value: 'six_month' },
    { label: '12 month', value: 'twelwe_month' },
    { label: '2 year', value: 'two_year' }
]);

const productService = new ProductService();

onBeforeMount(() => {
    initFilters();
});
onMounted(() => {
    productService.getProducts().then((data) => (products.value = data));
});

const openNew = () => {
    product.value = {};
    submitted.value = false;
    productDialog.value = true;
};

const hideDialog = () => {
    productDialog.value = false;
    submitted.value = false;
};

const saveProduct = () => {
    submitted.value = true;
    if (product.value.first_name && product.value.first_name && product.value.email && product.value.registration && product.value.dateOfBirth && product.value.loan_amount && product.value.credit_score) {
        if (product.value.id) {
            product.value.inventoryStatus = product.value.inventoryStatus.value ? product.value.inventoryStatus.value : product.value.inventoryStatus;
            products.value[findIndexById(product.value.id)] = product.value;
            toast.add({ severity: 'success', summary: 'Successful', detail: 'Product Updated', life: 3000 });
        } else {
            product.value.id = createId();
            product.value.code = createId();
            product.value.image = 'product-placeholder.svg';
            product.value.inventoryStatus = product.value.inventoryStatus ? product.value.inventoryStatus.value : '1 month';
            products.value.push(product.value);
            toast.add({ severity: 'success', summary: 'Successful', detail: 'Product Created', life: 3000 });
        }
        productDialog.value = false;
        product.value = {};
    }
};

const editProduct = (editProduct) => {
    product.value = { ...editProduct };
    console.log(product);
    productDialog.value = true;
};

const confirmDeleteProduct = (editProduct) => {
    product.value = editProduct;
    deleteProductDialog.value = true;
};

const deleteProduct = () => {
    products.value = products.value.filter((val) => val.id !== product.value.id);
    deleteProductDialog.value = false;
    product.value = {};
    toast.add({ severity: 'success', summary: 'Successful', detail: 'Product Deleted', life: 3000 });
};

const findIndexById = (id) => {
    let index = -1;
    for (let i = 0; i < products.value.length; i++) {
        if (products.value[i].id === id) {
            index = i;
            break;
        }
    }
    return index;
};

const createId = () => {
    let id = '';
    const chars = '0123456789';
    for (let i = 0; i < 5; i++) {
        id += chars.charAt(Math.floor(Math.random() * chars.length));
    }
    return id;
};

const exportCSV = () => {
    dt.value.exportCSV();
};

const confirmDeleteSelected = () => {
    deleteProductsDialog.value = true;
};
const deleteSelectedProducts = () => {
    products.value = products.value.filter((val) => !selectedProducts.value.includes(val));
    deleteProductsDialog.value = false;
    selectedProducts.value = null;
    toast.add({ severity: 'success', summary: 'Successful', detail: 'Products Deleted', life: 3000 });
};

const initFilters = () => {
    filters.value = {
        global: { value: null, matchMode: FilterMatchMode.CONTAINS }
    };
};
</script>

<template>
    <div class="grid" style="">
        <div class="col-12">
            <div class="card">
                <Toast />
                <Toolbar class="mb-4">
                    <template v-slot:start>
                        <div class="my-2">
                            <Button label="New" icon="pi pi-plus" class="p-button-success mr-2" @click="openNew" />
                            <Button label="Delete" icon="pi pi-trash" class="p-button-danger" @click="confirmDeleteSelected" :disabled="!selectedProducts || !selectedProducts.length" />
                        </div>
                    </template>

                    <template v-slot:end>
                        <FileUpload mode="basic" accept="image/*" :maxFileSize="1000000" label="Import" chooseLabel="Import" class="mr-2 inline-block" />
                        <Button label="Export" icon="pi pi-upload" class="p-button-help" @click="exportCSV($event)" />
                    </template>
                </Toolbar>

                <DataTable
                    ref="dt"
                    :value="products"
                    v-model:selection="selectedProducts"
                    dataKey="id"
                    :paginator="true"
                    :rows="10"
                    :filters="filters"
                    paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown"
                    :rowsPerPageOptions="[5, 10, 25]"
                    currentPageReportTemplate="Showing {first} to {last} of {totalRecords} products"
                    responsiveLayout="scroll"
                >
                    <template #header>
                        <div class="flex flex-column md:flex-row md:justify-content-between md:align-items-center">
                            <h5 class="m-0">Manage Products</h5>
                            <span class="block mt-2 md:mt-0 p-input-icon-left">
                                <i class="pi pi-search" />
                                <InputText v-model="filters['global'].value" placeholder="Search..." />
                            </span>
                        </div>
                    </template>

                    <Column selectionMode="multiple" headerStyle="width: 3rem"></Column>
                    <Column field="code" header="Code" :sortable="true" headerStyle="width:14%; min-width:10rem;">
                        <template #body="slotProps">
                            <span class="p-column-title">Code</span>
                            {{ slotProps.data.id }}
                        </template>
                    </Column>
                    <Column field="first_name" header="First Name" :sortable="true" headerStyle="width:14%; min-width:10rem;">
                        <template #body="slotProps">
                            <span class="p-column-title">first_name</span>
                            {{ slotProps.data.first_name }}
                        </template>
                    </Column>
                    <Column field="last_name" header="Last Name" :sortable="true" headerStyle="width:14%; min-width:10rem;">
                        <template #body="slotProps">
                            <span class="p-column-title">last_name</span>
                            {{ slotProps.data.last_name }}
                        </template>
                    </Column>

                    <Column field="leasing_amount" header="Amount" :sortable="true" headerStyle="width:14%; min-width:8rem;">
                        <template #body="slotProps">
                            <span class="p-column-title">amount</span>
                            {{ slotProps.data.loan_amount }}
                        </template>
                    </Column>
                    <Column field="em_status" header="Employment" :sortable="true" headerStyle="width:14%; min-width:10rem;">
                        <template #body="slotProps">
                            <span class="p-column-title">em_status</span>
                            {{ slotProps.data.category }}
                        </template>
                    </Column>
                    <Column field="rating" header="Reviews" :sortable="true" headerStyle="width:14%; min-width:10rem;">
                        <template #body="slotProps">
                            <span class="p-column-title">Rating</span>
                            <Rating :modelValue="slotProps.data.rating" :readonly="true" :cancel="false" />
                        </template>
                    </Column>
                    <Column field="inventoryStatus" header="Status" :sortable="true" headerStyle="width:14%; min-width:10rem;">
                        <template #body="slotProps">
                            <span class="p-column-title">Status</span>
                            <span :class="'product-badge status-' + (slotProps.data.inventoryStatus ? slotProps.data.inventoryStatus.toLowerCase() : '')">{{ slotProps.data.inventoryStatus }}</span>
                        </template>
                    </Column>
                    <Column headerStyle="min-width:10rem;">
                        <template #body="slotProps">
                            <Button icon="pi pi-pencil" class="p-button-rounded p-button-success mr-2" @click="editProduct(slotProps.data)" />
                            <Button icon="pi pi-trash" class="p-button-rounded p-button-warning mt-2" @click="confirmDeleteProduct(slotProps.data)" />
                        </template>
                    </Column>
                </DataTable>

                <Dialog v-model:visible="productDialog" :style="{ width: '600px' }" header="Product Details" :modal="true" class="p-fluid">
                    <img :src="'demo/images/product/' + product.image" :alt="product.image" v-if="product.image" width="150" class="mt-0 mx-auto mb-5 block shadow-2" />

                    <!-- input name -->
                    <div class="container">
                        <div class="field">
                            <label for="first_name">First Name</label>
                            <InputText id="first_name" v-model.trim="product.first_name" required="true" autofocus :class="{ 'p-invalid': submitted && !product.first_name }" />
                            <small class="p-invalid" v-if="submitted && !product.first_name">first name is required.</small>
                        </div>

                        <div class="field">
                            <label for="last_name">Last Name</label>
                            <InputText id="last_name" v-model.trim="product.last_name" required="true" autofocus :class="{ 'p-invalid': submitted && !product.last_name }" />
                            <small class="p-invalid" v-if="submitted && !product.last_name">last name is required.</small>
                        </div>
                    </div>

                    <!-- email and registration number -->
                    <div class="container">
                        <div class="field">
                            <label for="email">Email</label>
                            <InputText id="email" v-model.trim="product.email" required="true" autofocus :class="{ 'p-invalid': submitted && !product.email }" />
                            <small class="p-invalid" v-if="submitted && !product.email">email is required.</small>
                        </div>

                        <div class="field">
                            <label for="registration">Registration</label>
                            <InputText id="registration" v-model.trim="product.registration" required="true" autofocus :class="{ 'p-invalid': submitted && !product.registration }" />
                            <small class="p-invalid" v-if="submitted && !product.registration">Registration name is required.</small>
                        </div>
                    </div>

                    <div class="container">
                        <div class="field">
                            <label for="dateOfBirth">Date of Birth</label>
                            <Calendar :showIcon="true" :showButtonBar="true" v-model="calendarValue"></Calendar>
                        </div>
                        <div class="field">
                            <label for="loan_amount">Loan amount</label>
                            <InputText id="loan_amount" v-model.trim="product.loan_amount" required="true" autofocus :class="{ 'p-invalid': submitted && !product.loan_amount }" />
                            <small class="p-invalid" v-if="submitted && !product.loan_amount">loan_amount name is required.</small>
                        </div>
                    </div>

                    <div class="field">
                        <label class="mb-3">Category</label>
                        <div class="formgrid grid">
                            <div class="field-radiobutton col-6">
                                <RadioButton id="category1" name="category" value="Employed" v-model="product.category" />
                                <label for="category1">Employed</label>
                            </div>
                            <div class="field-radiobutton col-6">
                                <RadioButton id="category2" name="category" value="Self-Employed" v-model="product.category" />
                                <label for="category2">Self-Employed</label>
                            </div>
                            <div class="field-radiobutton col-6">
                                <RadioButton id="category3" name="category" value="Unemployed" v-model="product.category" />
                                <label for="category3">Unemployed</label>
                            </div>
                            <div class="field-radiobutton col-6">
                                <RadioButton id="category4" name="category" value="Retired" v-model="product.category" />
                                <label for="category4">Retired</label>
                            </div>
                        </div>
                    </div>

                    <div class="container">
                        <div class="field">
                            <label for="inventoryStatus">Status</label>
                            <Dropdown id="inventoryStatus" v-model="product.inventoryStatus" :options="statuses" optionLabel="label" placeholder="Select a Status">
                                <template #value="slotProps">
                                    <div v-if="slotProps.value && slotProps.value.value">
                                        <span :class="'product-badge status-' + slotProps.value.value">{{ slotProps.value.label }}</span>
                                    </div>
                                    <div v-else-if="slotProps.value && !slotProps.value.value">
                                        <span :class="'product-badge status-' + slotProps.value.toLowerCase()">{{ slotProps.value }}</span>
                                    </div>
                                    <span v-else>
                                        {{ slotProps.placeholder }}
                                    </span>
                                </template>
                            </Dropdown>
                        </div>
                        <div class="field">
                            <label for="credit_score">Credit Score</label>
                            <InputText id="credit_score" v-model.trim="product.credit_score" required="true" autofocus :class="{ 'p-invalid': submitted && !product.credit_score }" />
                            <small class="p-invalid" v-if="submitted && !product.credit_score">credit_score name is required.</small>
                        </div>
                    </div>

                    <div class="field">
                        <label for="description">Description</label>
                        <Textarea id="description" v-model="product.description" required="true" rows="3" cols="20" />
                    </div>
                    <!-- <div class="formgrid grid">
                        <div class="field col">
                            <label for="price">Price</label>
                            <InputNumber id="price" v-model="product.price" mode="currency" currency="USD" locale="en-US" :class="{ 'p-invalid': submitted && !product.price }" :required="true" />
                            <small class="p-invalid" v-if="submitted && !product.price">Price is required.</small>
                        </div>
                        <div class="field col">
                            <label for="quantity">Quantity</label>
                            <InputNumber id="quantity" v-model="product.quantity" integeronly />
                        </div>
                    </div> -->

                    <template #footer>
                        <Button label="Cancel" icon="pi pi-times" class="p-button-text" @click="hideDialog" />
                        <Button label="Save" icon="pi pi-check" class="p-button-text" @click="saveProduct" />
                    </template>
                </Dialog>

                <Dialog v-model:visible="deleteProductDialog" :style="{ width: '450px' }" header="Confirm" :modal="true">
                    <div class="flex align-items-center justify-content-center">
                        <i class="pi pi-exclamation-triangle mr-3" style="font-size: 2rem" />
                        <span v-if="product"
                            >Are you sure you want to delete <b>{{ product.first_name }}</b
                            >?</span
                        >
                    </div>
                    <template #footer>
                        <Button label="No" icon="pi pi-times" class="p-button-text" @click="deleteProductDialog = false" />
                        <Button label="Yes" icon="pi pi-check" class="p-button-text" @click="deleteProduct" />
                    </template>
                </Dialog>

                <Dialog v-model:visible="deleteProductsDialog" :style="{ width: '450px' }" header="Confirm" :modal="true">
                    <div class="flex align-items-center justify-content-center">
                        <i class="pi pi-exclamation-triangle mr-3" style="font-size: 2rem" />
                        <span v-if="product">Are you sure you want to delete the selected products?</span>
                    </div>
                    <template #footer>
                        <Button label="No" icon="pi pi-times" class="p-button-text" @click="deleteProductsDialog = false" />
                        <Button label="Yes" icon="pi pi-check" class="p-button-text" @click="deleteSelectedProducts" />
                    </template>
                </Dialog>
            </div>
        </div>
    </div>
</template>

<style scoped lang="scss">
.container {
    display: flex;
    justify-content: space-between;
}

.field {
    flex: 1;
    margin-right: 10px; /* Add spacing between the fields */
}
</style>
