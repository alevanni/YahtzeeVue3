
<script setup>

// component that displays thrown dice and 
// contains the button to throw them

import { ref } from 'vue'

const props = defineProps({
    count: {
        type: Object
    }
});
const count = props.count;

const dice = {
    1: '&#x2680',
    2: '&#x2681',
    3: '&#x2682',
    4: '&#x2683',
    5: '&#x2684',
    6: '&#x2685'
}
//console.log('count:', count)

const thisRound = ref([1, 2, 3, 4, 5])

const  throwFiveTimes = () => {

    // we reset count to zero
    for (let i = 1; i < 7; i++) {
        count[i] = 0;
    }

    // we throw the dice five times
    for (let i = 0; i < 5; i++) {
        thisRound.value[i] = Math.floor(Math.random() * 6 + 1);
    }
    
    // we count
    thisRound.value.forEach( element => {
        count[element] += 1;
    });
    
    console.log('result: ' + thisRound.value) 
} 

</script>
 
<template>
    <div class="dice-board">
        <table id="dice">
                <tr>
                    <td class="die" v-for="result in thisRound" v-html="dice[result] "></td>
                    
                </tr>
        </table>
            <!-- button that throws the dice -->
            <button id="gooi" @click="throwFiveTimes">Throw!</button>
    </div>
</template>