<script lang="ts">
  import svelteLogo from "./assets/svelte.svg";
  import viteLogo from "/vite.svg";
  import Counter from "./lib/Counter.svelte";

  async function getQuizzes() {
    let response = await fetch("http://localhost:3000/api/quizzes");

    if (!response.ok) {
      alert("failed");
      return;
    }

    let jsonResponse = await response.json();
    console.log(jsonResponse);
  }

  function connect() {
    let websocket = new WebSocket("ws://localhost:3000/ws");
    websocket.onopen = () => {
      console.log("Connection opened");
      websocket.send("Hello World")
    };

    websocket.onmessage = (event) => {
      console.log(event.data)
    }
  }
</script>

<button on:click={getQuizzes}>Get quizzes</button>
<button on:click={connect}>Connect</button>

<main>

</main>
<style>
  button {
    border:solid 1px black;
  }
</style>
