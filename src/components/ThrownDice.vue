
<script setup>

// component that displays thrown dice and 
// contains the button to throw them

import { ref } from 'vue'



</script>

<template>
    <div class="dice-board">
        <table id="dice">
                <tr>
                    <td class="die">&#x2680;</td>
                    <td class="die">&#x2681;</td>
                    <td class="die">&#x2682;</td>
                    <td class="die">&#x2683;</td>
                    <td class="die">&#x2684;</td>
                </tr>
            </table>
            <!-- button that throws the dice -->
            <button id="gooi">Throw!</button>
    </div>
</template>