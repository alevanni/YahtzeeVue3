<script setup>

// component that displays the score tables

import { ref, reactive, computed } from 'vue'
const props = defineProps({
    dice: {
        type: Array
    }
});
const explanations = {
    1: 'Total sum of &#x2680',
    2: 'Total sum of &#x2681',
    3: 'Total sum of &#x2682',
    4: 'Total sum of &#x2683',
    5: 'Total sum of &#x2684',
    6: 'Total sum of &#x2685'
}

const count = computed( () => {
    let c = {
        1: 0,
        2: 0,
        3: 0,
        4: 0,
        5: 0,
        6: 0
    }
    // we have thrown at least once
    if (props.dice[0] !== 0) {
        props.dice.forEach(element => {
        c[element] += 1;
    });
    }
    
    return c;
});

const pipSums = computed( () => {
    let pips = {
        1: 0,
        2: 0,
        3: 0,
        4: 0,
        5: 0,
        6: 0
    }
    
    for (let key in count.value) {
        pips[key] = key * count.value[key];
    }
    return pips;
}

);

const totalPipSum = computed( () => {
    let total = 0;
    for (let key in pipSums.value) {
        total += pipSums.value[key];
    }
    return total;
});

const bonus = computed( () => {
    
    return totalPipSum.value>=63 ? 35 : 0;

}
);


</script>

<template>
    <div class="score">
        <table class="score-table first">
        <thead>
            <tr>
                <th>Deel 1</th>
                <th>Explanation</th>
                <th>1st throw</th>
                <th>2nd throw</th>
                <th>3rd throw</th>
                <th>4th throw</th>
                <th>5th throw</th>
                <th>6th throw</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(result, key) in pipSums" >
                <td>{{ key }}</td>
                <td v-html="explanations[key]">  </td>
                <td>{{ result }}</td>
            </tr>
            <tr>
                <td>Total points</td>
                <td>&rarr;</td>
                <td>{{ totalPipSum }}</td>
            </tr>
            <tr>
                <td>Bonus</td>
                <td>35 points if sum is>=63</td>
                <td>{{ bonus }}</td>
            </tr>
            <tr>
                <td>Total</td>
                <td>of the first half &rarr;</td>
                <td>{{ totalPipSum + bonus}}</td>
            </tr>
        </tbody>
    
    </table>

    <table class="score-table second">
        <thead>
            <tr>
                <th>Deel 2</th>
                <th>Explanation</th>
                <th>1st throw</th>
                <th>2nd throw</th>
                <th>3rd throw</th>
                <th>4th throw</th>
                <th>5th throw</th>
                <th>6th throw</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Tris</td>
                <td class="explanation">Three of a kind: total score of the dice</td>
            </tr>
            <tr>
                <td>Carré</td>
                <td class="explanation">Four of a kind: total score of the dice</td>
            </tr>
            <tr>
                <td>Full House</td>
                <td class="explanation">Three and two of a kind: 25 points</td>
            </tr>
            <tr>
                <td>Small Straight</td>
                <td class="explanation">Sequence of four: 30 points</td>
            </tr>
            <tr>
                <td>Long Straight</td>
                <td class="explanation">Sequence of five: 40 points</td>
            </tr>
            <tr>
                <td>Yahtzee</td>
                <td class="explanation">Five of a kind: 50 points</td>
            </tr>
            <tr>
                <td>Chance</td>
                <td class="explanation">Total sum of all five dice</td>
            </tr>
            <tr>
                <td>Total</td>
                <td >of the top half</td>
            </tr>
            <tr>
                <td>Total</td>
                <td>of the bottom half</td>
            </tr>
            <tr>
                <td>Grand Total</td>
                <td>&rarr;</td>
            </tr>
        </tbody>
    
    </table>
    </div>
</template>