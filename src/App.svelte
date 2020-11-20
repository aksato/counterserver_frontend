<script>
  export let name;
  let counter = 0;
  let numUsers = 0;
  const ws = new WebSocket("ws://127.0.0.1:6789");

  ws.onopen = function () {
    console.log("Websocket client connected");
  };

  ws.onmessage = function (e) {
    let data = JSON.parse(e.data);
    if (data.type == "state") {
      counter = data.value;
    } else if (data.type == "users") {
      numUsers = data.count;
    } else {
      console.error("unsupported event", data);
    }
  };

  function handleMinusClick() {
    ws.send(JSON.stringify({ action: "minus" }));
  }

  function handlePlusClick() {
    ws.send(JSON.stringify({ action: "plus" }));
  }
</script>

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
  .buttons {
    font-size: 4em;
    display: flex;
    justify-content: center;
  }

  .button,
  .value {
    line-height: 1;
    padding: 2rem;
    margin: 2rem;
    border: medium solid;
    min-height: 1em;
    min-width: 1em;
  }

  .button {
    cursor: pointer;
    user-select: none;
  }

  .minus {
    color: red;
  }

  .plus {
    color: green;
  }

  .value {
    min-width: 2em;
  }

  .state {
    font-size: 2em;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <h1>Hello {name}!</h1>
  <p>
    Visit the
    <a href="https://svelte.dev/tutorial">Svelte tutorial</a>
    to learn how to build Svelte apps.
  </p>
  <div class="buttons">
    <button class="minus button" on:click={handleMinusClick}>-</button>
    <div class="value">{counter}</div>
    <button class="plus button" on:click={handlePlusClick}>+</button>
  </div>
  <div class="state">
    <span class="users">{numUsers} {numUsers > 1 ? 'users' : 'user'}</span>
    online
  </div>
</main>
