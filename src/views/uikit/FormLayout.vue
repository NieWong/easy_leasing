<script setup>
import { ref } from 'vue';

const formData = ref({
    firstName: '', 
    lastName: '',  
    email: '',
    loanAmount: null,
    loanPurpose: '',
    employmentStatus: null,
    income: null,
    creditScore: null,
    loanTerm: null,
    comments: ''
});

const employmentStatusOptions = ref([
    { label: 'Employed', value: 'employed' },
    { label: 'Self-Employed', value: 'selfEmployed' },
    { label: 'Unemployed', value: 'unemployed' },
    { label: 'Retired', value: 'retired' }
]);

const submitForm = () => {
    console.log('Form data:', formData.value);
};

const calendarValue = ref(null); 

</script>

<template>
    <div class="grid" style="margin-top: 1rem;">
        <div class="card" style="padding: 4rem; width: 700px;">
            <h5>Loan Application Form</h5>
            <div class="p-fluid formgrid grid">
                <div class="field col-12 md:col-6">
                    <label for="firstName">First Name</label>
                    <input id="firstName" v-model="formData.firstName" type="text" class="p-inputtext" />
                </div>
                <div class="field col-12 md:col-6">
                    <label for="lastName">Last Name</label>
                    <input id="lastName" v-model="formData.lastName" type="text" class="p-inputtext" />
                </div>
                <div class="field col-12 md:col-6">
                    <label for="email">Email</label>
                    <input id="email" v-model="formData.email" type="text" class="p-inputtext" />
                </div>
                <div class="field col-12 md:col-6">
                    <label for="dateOfBirth">Date of Birth</label>
                    <Calendar :showIcon="true" :showButtonBar="true" v-model="calendarValue"></Calendar>
                </div>
                <div class="field col-12">
                    <label for="loanAmount">Loan Amount ($)</label>
                    <input id="loanAmount" v-model="formData.loanAmount" type="number" class="p-inputtext" />
                </div>
                <div class="field col-12">
                    <label for="loanPurpose">Loan Purpose</label>
                    <input id="loanPurpose" v-model="formData.loanPurpose" type="text" class="p-inputtext" />
                </div>
                <div class="field col-12">
                    <label for="employmentStatus">Employment Status</label>
                    <Dropdown id="employmentStatus" v-model="formData.employmentStatus" :options="employmentStatusOptions" optionLabel="label" placeholder="Select One"></Dropdown>
                </div>
                <div class="field col-12">
                    <label for="income">Monthly Income ($)</label>
                    <input id="income" v-model="formData.income" type="number" class="p-inputtext" />
                </div>
                <div class="field col-12">
                    <label for="creditScore">Credit Score</label>
                    <input id="creditScore" v-model="formData.creditScore" type="number" class="p-inputtext" />
                </div>
                <div class="field col-12">
                    <label for="loanTerm">Loan Term (months)</label>
                    <input id="loanTerm" v-model="formData.loanTerm" type="number" class="p-inputtext" />
                </div>
                <div class="field col-12">
                    <label for="comments">Additional Comments</label>
                    <textarea id="comments" v-model="formData.comments" rows="3" class="p-inputtextarea"></textarea>
                </div>
                <div class="field col-12">
                    <Button label="Submit" @click="submitForm" />
                </div>
            </div>
        </div>
    </div>
</template>
