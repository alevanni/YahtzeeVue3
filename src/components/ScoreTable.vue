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
    let occurrencies = {
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
        occurrencies[element] += 1;
    });
    }
    console.log(occurrencies);
    return occurrencies;
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

const topTotal = computed( () => {
    return totalPipSum.value + bonus.value;
});

const tris = computed( () => {
    let values = Object.values(count.value);
    if (values.find((element) => element == 3) && !values.find((element) => element == 2 ) ) return totalPipSum.value;
    else return 0;
});

const carre = computed( () => {
    
    let values = Object.values(count.value);
    
    if (values.find((element) => element == 4)) {
        return totalPipSum.value;
    }
    else return 0;
});

const fullHouse = computed( () => {
    let values = Object.values(count.value);
    if (values.find((element) => element == 3) && values.find((element) => element == 2 ) ) return 25;
    else return 0;
});

const smallStraight = computed( () => {
    //if we have long straight, it's zero
    if (longStraight.value > 0 ) return 0;
    //three options: result contains 1234, 2345, 3456
    //we check if it contains 3 and 4,otherwise it's not possible
    if (count.value[3] > 0 && count.value[4] > 0) {
        if (count.value[1] > 0 && count.value[2] > 0) {//we check for 1 and 2
           
            return 30;
        }
        else if (count.value[2] > 0 && count.value[5] > 0) { //we check for 2 and 5
            
            return 30;
        }
        else if (count.value[5] > 0 && count.value[6] > 0) { //we check for 5 and 6
            
            return 30;
        }
        else {
           
            return 0;
        }
    }

    else {
       
        return 0;
    }
});

const longStraight = computed( () => {
    //grote straat: only two possible options, compare the values array [0,1,1,1,1,1] or [1,1,1,1,1,0]
    let values = Object.values(count.value);
    //we cannot compare two arrays, so we convert the count values to strings and then compare
    let valuesString = values.toString();
    
    if (valuesString === "0,1,1,1,1,1" || valuesString === "1,1,1,1,1,0") {
                
        return 40;
    }
    else {
        
        return 0;
    }
});

const yahtzee = computed( () => {
    let values = Object.values(count.value);
    if (values.find((element) => element == 5)) {
        return 50;
    }
    else {
        return 0;
    }

});

const bottomTotal = computed( () => {
    return tris.value + carre.value + fullHouse.value + smallStraight.value + longStraight.value + yahtzee.value + totalPipSum.value;
});

const grandTotal = computed( () => {
    return topTotal.value + bottomTotal.value;
});

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
                <td>{{ topTotal }}</td>
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
                <td>{{ tris }}</td>
            </tr>
            <tr>
                <td>Carré</td>
                <td class="explanation">Four of a kind: total score of the dice</td>
                <td>{{ carre }}</td>
            </tr>
            <tr>
                <td>Full House</td>
                <td class="explanation">Three and two of a kind: 25 points</td>
                <td>{{ fullHouse }}</td>
            </tr>
            <tr>
                <td>Small Straight</td>
                <td class="explanation">Sequence of four: 30 points</td>
                <td>{{ smallStraight }}</td>
            </tr>
            <tr>
                <td>Long Straight</td>
                <td class="explanation">Sequence of five: 40 points</td>
                <td>{{ longStraight }}</td>
            </tr>
            <tr>
                <td>Yahtzee</td>
                <td class="explanation">Five of a kind: 50 points</td>
                <td>{{ yahtzee }}</td>
            </tr>
            <tr>
                <td>Chance</td>
                <td class="explanation">Total sum of all five dice</td>
                <td>{{ totalPipSum }}</td>
            </tr>
            <tr>
                <td>Total</td>
                <td >of the top half</td>
                <td>{{ topTotal }}</td>
            </tr>
            <tr>
                <td>Total</td>
                <td>of the bottom half</td>
                <td>{{ bottomTotal }}</td>
            </tr>
            <tr>
                <td>Grand Total</td>
                <td>&rarr;</td>
                <td>{{ grandTotal }}</td>
            </tr>
        </tbody>
    
    </table>
    </div>
</template>