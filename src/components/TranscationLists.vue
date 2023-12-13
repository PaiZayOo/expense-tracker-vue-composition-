<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount > 0 ? 'plus' : 'minus'">
          {{transaction.text}}<span>${{transaction.amount}}</span><button @click="deleteTranscation(transaction.id)" class="delete-btn">x</button>
        </li>
    </ul>
</template>

<script setup>
import { defineProps } from 'vue';
import { useToast } from 'vue-toastification'

const toast = useToast();
const  props = defineProps({
    transactions : {
        type : Array,
        required : true
    }
});
const emit = defineEmits(['deleteSubmit']);

const deleteTranscation = (id) => {
    emit('deleteSubmit',id);
    toast.success('transcation deleted');
    
}

</script>