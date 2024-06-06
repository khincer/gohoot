<script lang="ts">
  import Button from "./lib/Button.svelte";
  import QuizCard from "./lib/QuizCard.svelte";

  let quizzes: { _id: string; name: string }[] = [];
  async function getQuizzes() {
    let response = await fetch("http://localhost:3000/api/quizzes");

    if (!response.ok) {
      alert("failed");
      return;
    }

    let jsonResponse = await response.json();
    quizzes = jsonResponse;
  }

  function connect() {
    let websocket = new WebSocket("ws://localhost:3000/ws");
    websocket.onopen = () => {
      console.log("Connection opened");
      websocket.send("Hello World");
    };

    websocket.onmessage = (event) => {
      console.log(event.data);
    };
  }
</script>

<button on:click={getQuizzes}>Get quizzes</button>
<button on:click={connect}>Connect</button>
<Button>Test button</Button>

<main>
  {#each quizzes as quiz}
  <QuizCard {quiz}/>
  {/each}
</main>

<style>
  button {
    border: solid 1px black;
  }
</style>
