<script>
    window.digitalData = {
        pages: {
            pageInfo: {
                pageName: "Home Page",
            },
            attributes: {
                score: 0,
            },
        },
    };

    const local = window.sessionStorage;
    const checkId = () => {
        var newID = Math.floor(Math.random() * 10000);
        var existID = JSON.parse(local.getItem("id"));
        if (existID) {
            return true;
        } else {
            existID = JSON.stringify(newID);
            local.setItem("id", existID);
            return false;
        }
    };

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
            window.clearInterval(inter);
            fail = true;
            failMessage();
        }
    };

    //Check on correct btn click if display solution matches real solution
    const checkSolution = () => {
        if (count == solution) {
            console.log(window.digitalData);
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
            console.log(window.digitalData);
            createRandomNumbers();
            score += 1;
            time = 3;
        } else {
            failMessage();
            console.log("Fail");
        }
    };

    const reset = () => {
        window.clearInterval(inter);
        play();
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

        window.digitalData.pages.attributes = {
            score: score,
        };

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
            case "play":
                play();
                break;
            default:
                console.log("Error");
        }
    };

    const play = () => {
        createRandomNumbers();
        time = 3;
        inter = window.setInterval(() => {
            time -= 1;
            return checkTime();
        }, 1000);
        fail = false;
        score = 0;
    };
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
            <button on:click={handleClick} id="incorrect" value="incorrect"
                >Incorrect</button
            >
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
        background-image: linear-gradient(to left, #9effdf, #53ffc6);
        padding: 1em;
        box-shadow: 1px 3px 3px rgba(25, 243, 25, 0.5);
        color: #111;
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

    button {
        text-align: center;
        width: 40%;
        height: 55px;
        border: none;
        border-radius: 5px;
        box-shadow: 2px 2px 3px rgb(57, 255, 163);
        background-image: linear-gradient(to left, #9effdf, #16fcaf);
        font-weight: 600;
        font-size: 1.2em;
    }
    .reset-btn {
        box-shadow: 2px 2px 3px #f2ff39;
        background-image: linear-gradient(to left, #edff9e, #fff45d);
    }

    button#incorrect {
        background-image: linear-gradient(to left, #ff9ba0, #ff0a2b);
        box-shadow: 2px 2px 3px #ff7a71;
    }

    @media (min-width: 640px) {
        main {
            max-width: none;
        }
    }
</style>
