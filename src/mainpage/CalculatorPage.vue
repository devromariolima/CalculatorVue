<template>
    <div class="calculator">
        <Display :value="displayValue" />
        <Button label="AC" triple @onCalcButtonClick="clearMemory" />
        <Button label="/" operation @onCalcButtonClick="setOperation" />
        <Button label="7" @onCalcButtonClick="addDigit" />
        <Button label="8" @onCalcButtonClick="addDigit" />
        <Button label="9" @onCalcButtonClick="addDigit" />
        <Button label="*" operation @onCalcButtonClick="setOperation" />
        <Button label="4" @onCalcButtonClick="addDigit" />
        <Button label="5" @onCalcButtonClick="addDigit" />
        <Button label="6" @onCalcButtonClick="addDigit" />
        <Button label="-" operation @onCalcButtonClick="setOperation" />
        <Button label="1" @onCalcButtonClick="addDigit" />
        <Button label="2" @onCalcButtonClick="addDigit" />
        <Button label="3" @onCalcButtonClick="addDigit" />
        <Button label="+" operation @onCalcButtonClick="setOperation" />
        <Button label="0" double @onCalcButtonClick="addDigit" />
        <Button label="." @onCalcButtonClick="addDigit" />
        <Button label="=" operation @onCalcButtonClick="setOperation" />
    </div>
</template>

<script>

import Button from '../components/ButtonComponent.vue';
import Display from '../components/DisplayComponent.vue';

export default {
    data() {
        return {
            displayValue: "0",
            clearDisplay: false,
            operation: null,
            values: [0, 0],
            current: 0
        }
    },
    components: { Display, Button },
    methods: {
        clearMemory() {
            Object.assign(this.$data, this.$options.data())
        },
        setOperation(operation) {
            if (this.current === 0) {
                this.operation = operation
                this.current = 1
                this.clearDisplay = true
            } else {
                const equals = operation === "="
                const currentOperation = this.operation

                try {
                    this.values[0] = eval(`${this.values[0]} ${currentOperation} ${this.values[1]}`
                    )
                    if (isNaN(this.values[0]) || !isFinite(this.values[0])) {
                        this.clearMemory()
                        return
                    }
                } catch (e) {
                    this.$emit("onRrror", e)
                }
                this.values[1] = 0
                this.displayValue = this.values[0]
                this.operation = equals ? null : operation
                this.current = equals ? 0 : 1
                this.clearDisplay = !equals
            }
        },
        addDigit(n) {
            if (n == "." && this.displayValue.includes(".")) {
                return
            }

            const clearDisplay = this.displayValue === "0" || this.clearDisplay
            const currentValue = clearDisplay ? "" : this.displayValue
            const displayValue = currentValue + n
            this.displayValue = displayValue
            this.clearDisplay = false
            this.values[this.current] = displayValue

            if (n !== ".") {
                const i = this.current
                const newValue = parseFloat(displayValue)
                this.values[i] = newValue
            }

        },
    }

}
</script>

<style>
.calculator {

    height: 320px;
    width: 235px;
    border-radius: 5px;
    overflow: hidden;
    display: grid;
    grid-template-columns: repeat(4, 25%);
    grid-template-rows: 1fr 48px 48px 48px 48px 48px;
}
</style>