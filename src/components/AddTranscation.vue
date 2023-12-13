<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="AddTranscation">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" placeholder="Enter text..." v-model="text" />
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
                (negative - expense, positive - income)</label>
            <input type="number" id="amount" placeholder="Enter amount..." v-model="amount" />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>
<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification'

const text = ref(null);
const amount = ref(null);
const toast = useToast();

const emit = defineEmits(['transcationSubmitted'])
const AddTranscation = () => {
    if (!text.value || !amount.value) {
        toast.error('Both fields must be filled');
        return;
    }
    const transactionData = {
        text: text.value,
        amount: amount.value
    };

    emit('transcationSubmitted', transactionData);
    toast.success('Transcation success')
    text.value = '';
    amount.value = '';
}
</script>