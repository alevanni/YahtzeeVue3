<script setup>

// component that displays the score tables

import { ref } from 'vue'
const props = defineProps({
    count: {
        type: Object
    }
});
const count = props.count;


</script>

<template>
    <div class="score-table">
        <table>
        <thead>
            <tr>
                <th>Getal</th>
                <th>Aantal</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(result, key) in count" ><td>{{ key }}</td><td>{{ result }}</td></tr>
            
        </tbody>
    
    </table>
    </div>
</template>