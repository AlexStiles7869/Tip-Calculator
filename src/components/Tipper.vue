<script lang="ts">
import Vue from 'vue'
export default {
    name: "Tipper",
    data() {
        return {
            tip_options: [5,10,15,25,50] as number[],
            
            bill: 0 as number,
            tip_percentage: 5 as number,
            people: 1 as number,

            tip_amount_per_person: 0 as number,
            total_bill_per_person: 0 as number,
        }
    }, methods: {
        ready_to_calculate(): boolean {            
            return this.bill && this.tip_percentage && this.people;
        },

        calculate_tip_bill_per_person(): number {
            return (this.bill * (this.tip_percentage / 100)) / this.people;
        },

        calculate_total_bill_per_person(): number {
            return (this.bill / this.people) + this.calculate_tip_bill_per_person();
        },

        calculate_tip_values(): void {
            if (!this.ready_to_calculate()) {
                return;
            }

            this.tip_amount_per_person = this.calculate_tip_bill_per_person();
            this.total_bill_per_person = this.calculate_total_bill_per_person();
        },

        reset() {
            console.log("here")
            this.bill = this.tip_percentage = this.people = 0;
        }
    }
}
</script>

<template>
    <div class="tipper-container">
        <div class="tipper-column">
            <div class="tipper-input-container">
                <label for="bill-input">Bill</label>
                <input name="bill-input" type="number" required v-model.number="bill" v-on:keyup="calculate_tip_values">
            </div>
            <div class="tip-percentage-select">
                <label for="">Select Tip %</label>
                <div>
                    <button class="tip-percentage-button" v-for="tip_option in tip_options" :key="tip_option">{{tip_option}}%</button>
                    <input class="tip-percentage-custom" type="number" placeholder="Custom" v-model.number="tip_percentage">
                </div>
            </div>
            <div class="tipper-input-container">
                <label for="people-input">People</label>
                <input name="people-input" type="number" required v-model.number="people" v-on:keyup="calculate_tip_values">
            </div>
        </div>
        <div class="tipper-column tip-result">
            <div class="result-section-container">
                <div class="result-section-details">
                    <h3>Tip Amount</h3>
                    <span class="result-subheading">/ person</span>
                </div>
                <span class="result-text">${{tip_amount_per_person.toFixed(2)}}</span>
            </div>
            <div class="result-section-container">
                <div class="result-section-details">
                    <h3>Total</h3>
                    <span class="result-subheading">/ person</span>
                </div>
                <span class="result-text">${{total_bill_per_person.toFixed(2)}}</span>
            </div>
            <form class="tip-reset-form" action="">
                <input class="tip-reset-button disabled" type="button" value="RESET" :on-submit="reset">
            </form>
        </div>
    </div>
</template>

<style scoped>
* {
    font-family: "Space Mono", 'Courier New', Courier, monospace;
}

.tipper-container {
    min-width: 40rem;
    background-color: white;
    border-radius: 1rem;
    padding: 1.5rem;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 2rem;
}

.tipper-column {
    display: flex;
    flex-direction: column;
}

.tipper-column.tip-result {
    background-color: hsl(183, 100%, 15%);
    border-radius: 1rem;
    padding: 2rem;
}

.tip-result > .result-section-container:not(:first-child) {
    margin-top: 2rem;
}

.result-section-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.result-section-details h3 {
    font-size: 0.9rem;
    color: white;
    line-height: 1;
}

.result-section-details .result-subheading {
    font-size: 0.7rem;
    color: hsl(184, 14%, 56%);
    font-weight: bold;
    line-height: 1;
}

.result-section-container .result-text {
    font-size: 2.5rem;
    font-weight: bold;
    color: hsl(172, 67%, 45%);
}

.tip-reset-button {
    margin-top: 7rem;
    border: none;
    border-radius: 0.25rem;
    padding: 0.5rem 2rem;
    font-weight: bold;
    width: 100%;
    font-size: 1rem;
    color: hsl(183, 100%, 15%);
    background-color:  hsl(172, 67%, 45%);
    cursor: pointer;
    transition: background-color 200ms;
    position: relative;
}

.tip-reset-button.disabled::after {
    content: "";
    background-color: black;
    opacity: 0.5;
    position: absolute;
    display: block;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}

.tip-reset-button:hover {
    background-color: hsl(185, 41%, 84%);
}

</style>