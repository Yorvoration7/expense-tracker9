<template>
<h3>History </h3>
    <ul id="list" class="list">
<li v-for="transaction in transactions" 
 :key="transaction.id"
 :class="transaction.amount < 0 ? 'minus' : 'plus'"> 
<!-- {{ transaction.text }} -->
{{transaction.text}}<span>${{ transaction.amount}}</span>
<button  @click="deleteTrans(transaction.id)" class="delete-btn">x</button>
</li>
    </ul>
</template>

<script setup>
import {ref, defineProps} from 'vue';


const emit = defineEmits('transactionDeleted')
const props = defineProps({
    transactions: {
        type: Array,
        required: true
    }
})

function deleteTrans(id) {
    emit('transactionDeleted', id)
}



</script>
<style scoped>

</style>