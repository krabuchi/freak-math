<script>
    let count = 0;
    let firstNumber = 0;
    let secondNumber = 0;
    let solution = 0;
    let fail = false;
    let displayFail = "";
    let time = 3;
    let score = 0;

    //Create random numbers to fill and calculate solution
    const createRandomNumbers = () => {
        firstNumber = Math.floor(Math.random() * 20);
        secondNumber = Math.floor(Math.random() * 20);
        var c = firstNumber + secondNumber;
        solution = c;
        createSolutionArr(c);
    };

    //Create solution array and pick one for display solution
    const createSolutionArr = (num) => {
        let num1 = num + 2;
        let num2 = num - 2;
        let num3 = num + 1;
        let num4 = num - 1;
        let solArr = [];
        if (num % 2 === 0) {
            solArr.push(num1, num2, num);
        } else {
            solArr.push(num3, num4, num);
        }
        count = solArr[Math.floor(Math.random() * solArr.length)];
    };

    var inter = window.setInterval(() => {
        time -= 1;
        checkTime();
    }, 1000);

    const checkTime = () => {
        if (time < 1 && fail == false) {
            clearInterval(inter);
            fail = true;
            failMessage();
        }
    };

    //Check on correct btn click if display solution matches real solution
    const checkSolution = () => {
        if (count == solution) {
            createRandomNumbers();
            score += 1;
            time = 3;
        } else {
            failMessage();
            console.log("Fail");
        }
    };

    //Check on incorrect btn click if display solution does not match real solution
    const checkNotSolution = () => {
        if (count !== solution) {
            createRandomNumbers();
            score += 1;
            time = 3;
        } else {
            failMessage();
            console.log("Fail");
        }
    };

    const reset = () => {
        createRandomNumbers();
        time = 3;
        inter = window.setInterval(() => {
            time -= 1;
            checkTime();
        }, 1000);
        fail = false;
    };

    //If wrong btn click display Fail Message
    const failMessage = () => {
        fail = true;
        clearInterval(inter);
        const failArr = [
            "You Suck!",
            "Disgraceful",
            "Disappointment",
            "Practice Hard",
        ];

        displayFail = failArr[Math.floor(Math.random() * failArr.length)];
    };

    //Check if btn clicked was correct or incorrect
    const handleClick = (e) => {
        const { value } = e.target;
        switch (value) {
            case "correct":
                checkSolution(); // verify solution function for correct soln
                break;
            case "incorrect":
                checkNotSolution(); // verify solution function for incorrect soln
                break;
            case "reset":
                reset();
                break;
            default:
                console.log("Error");
        }
    };
    createRandomNumbers();
</script>

<section class="timer">
    <div>
        <small>Time Left</small>
        <h2>{time}</h2>
    </div>
    <div>
        <small>Score</small>
        <h2>{score}</h2>
    </div>
</section>

{#if fail}
    <section class="error">
        <p>{displayFail}</p>
    </section>
{:else}
    <section class="play">
        <p>{firstNumber} + {secondNumber}</p>
        <span> = </span>
        <p>{count}</p>
        <div>
            <button on:click={handleClick} value="correct">Correct</button>
            <button on:click={handleClick} value="incorrect">Incorrect</button>
        </div>
    </section>
{/if}

<section class="reset-section">
    <button class="reset-btn" on:click={handleClick} value="reset">Reset</button
    >
</section>

<style>
    section {
        text-align: center;
    }

    p {
        margin: 0;
        padding: 0;
    }

    section.timer {
        display: flex;
        justify-content: space-between;
        background: rgb(173, 173, 255);
        padding: 1em;
    }

    h2 {
        margin: 0;
        font-size: 3em;
    }

    section.play {
        height: 60%;
        margin-top: 1em;
    }

    section.play p {
        font-size: 5rem;
    }

    section.play span {
        font-size: 4rem;
    }

    section.play div {
        position: absolute;
        width: 100%;
        bottom: 150px;
        display: flex;
        justify-content: space-evenly;
    }

    .error {
        background-color: #ff3e00;
        color: azure;
        font-size: 3em;
        height: 60%;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .error p {
        margin: 0;
        padding: 0;
    }

    .reset-section {
        width: 100vw;
        display: flex;
        justify-content: center;
        align-items: center;
        position: absolute;
        bottom: 75px;
    }

    .reset-btn,
    button {
        text-align: center;
        width: 40%;
        height: 55px;
        border: none;
        border-radius: 5px;
        box-shadow: 2px 2px 3px rgb(173, 173, 255);
    }

    @media (min-width: 640px) {
        main {
            max-width: none;
        }
    }
</style>
