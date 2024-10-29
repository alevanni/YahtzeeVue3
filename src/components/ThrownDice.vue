
<script setup>

// component that displays thrown dice and 
// contains the button to throw them

import { ref } from 'vue'
const emit = defineEmits(['throwDice']);
const playRound = () => emit('throwDice', throwFiveTimes());

const props = defineProps({
    dice: {
        type: Array
    } /*,
    count: {
        type: Object
    }*/
});
// const dice = props.dice;
//const count = props.count;

const diceImages = {
    0: '&#127922;',
    1: '&#x2680',
    2: '&#x2681',
    3: '&#x2682',
    4: '&#x2683',
    5: '&#x2684',
    6: '&#x2685'
}



const  throwFiveTimes = () => {
    
    let round = [] 

    // we reset count to zero
    /*
    for (let i = 1; i < 7; i++) {
        count[i] = 0;
    }*/

    // we throw the dice five times
    for (let i = 0; i < 5; i++) {
        round[i] = Math.floor(Math.random() * 6 + 1);
    }
    
    // we count
    /*
    round.forEach( element => {
        count[element] += 1;
    });
    */
    //console.log('result: ' + dice)
    return round; 
} 

</script>
 
<template>
    <div class="dice-board">
        <table id="dice">
                <tr>
                    <td class="die" v-for="result in props.dice" v-html="diceImages[result] "></td>
                    
                </tr>
        </table>
            <!-- button that throws the dice -->
            <button id="gooi" @click="playRound">Throw!</button>
    </div>
</template>