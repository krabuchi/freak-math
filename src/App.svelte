<script>
    export let name;
    const handleClick = (e) => {
        const { value } = e.target;
        switch (value) {
            case "correct":
                console.log("Marvelous");
                break;
            case "incorrect":
                console.log("Atrocious");
                break;
            default:
                console.log("Error");
        }
    };
</script>

<main>
    {name}
    <button on:click={handleClick} value="correct">Correct</button>
    <button on:click={handleClick} value="incorrect">Incorrect</button>
</main>

<style>
    main {
        text-align: center;
        padding: 1em;
        max-width: 240px;
        margin: 0 auto;
    }

    h1 {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 4em;
        font-weight: 100;
    }

    @media (min-width: 640px) {
        main {
            max-width: none;
        }
    }
</style>
