<script>
    import {onMount} from "svelte"

    Array.prototype.sample = function () {
        return this[Math.floor(Math.random() * this.length)]
    }

    let steps = {
        0.000000001: "one billionth of a",
        0.000001: "one millionth of a",
        0.001: "one thousandth of a",
        1000: "thousand",
        1000000: "million",
        1000000000: "billion",
    }

    let units = {
        time: {
            1: "seconds",
            60: "minutes",
            3600: "hours",
            86400: "days",
            604800: "weeks",
            2628000: "months",
            31536000: "years",
        },
        length: {
            0.001: "millimeters",
            0.01: "centimeters",
            1: "meters",
            1000: "kilometers",
            149597870700: "astronomical units",
        },
        money: {
            1: "dollars",
            70000: "median household income",
            114000000000: "Net worth of Jeff Bezos",
        },
    }

    let currentQuestion = ""
    let currentType = "time"
    let questionStep = 0
    let questionUnit = 0

    let answerNumber = 0
    let answerUnit = 0

    let correctAnswer = ""

    function newQuestion() {
        currentType = Object.keys(units).sample()
        questionStep = Object.keys(steps).sample()
        questionUnit = Object.keys(units[currentType]).sample()
        currentQuestion = `How much is 1 ${steps[questionStep]} ${units[currentType][questionUnit]}`
        correctAnswer = ""
        answerNumber = 0
        answerUnit = 0
    }

    onMount(() => {
        newQuestion()
    })

    function checkAnswer() {
        let seconds = questionStep * questionUnit
        let answerSeconds = answerNumber * answerUnit
        console.log(questionStep, questionUnit, seconds)
        let closestUnit = Object.keys(units[currentType]).sort((a, b) => {
            return Math.abs(seconds - a) - Math.abs(seconds - b)
        })[0]
        console.log(closestUnit)

        correctAnswer = `The correct answer is: ${(
            seconds / closestUnit
        ).toFixed(1)} ${
            units[currentType][closestUnit]
        }. The answer was this much bigger than you thought: ${(
            seconds / answerSeconds
        ).toFixed(4)}.`
    }
</script>

<span>{currentQuestion}</span>
<input type="text" bind:value={answerNumber} />
<select bind:value={answerUnit}>
    {#each Object.keys(units[currentType]) as unit}
        <option value={unit}>{units[currentType][unit]}</option>
    {/each}
</select>
<button on:click={checkAnswer}>Check</button>
<div>{correctAnswer}</div>
<button on:click={newQuestion}>New question</button>
