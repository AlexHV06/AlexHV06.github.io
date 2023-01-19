<script>
  let clicks = 100;
  let multiplier = 1;
  let workers = [];
  let worker_multiplier = 1;

  let upgrades = [
    { cost: 15, name: "Doubler", multiplier: 2, worker: 0 },
    { cost: 50, name: "Worker", multiplier: 0, worker: 1 },
    { cost: 5000, name: "Super Worker", multiplier: 10, worker: 1 },
    { cost: 0, name: "Cheats", multiplier: 1000000000000, worker: 10000000000000 },
  ];

  function increment() {
    clicks = clicks + multiplier;
    console.log("click" + clicks);
  }

  function nFormatter(num, digits) {
  const lookup = [
    { value: 1, symbol: "" },
    { value: 1e3, symbol: "k" },
    { value: 1e6, symbol: "M" },
    { value: 1e9, symbol: "B" },
    { value: 1e12, symbol: "T" },
    { value: 1e15, symbol: "Q" },
    { value: 1e18, symbol: "Quin" },
    { value: 1e21, symbol: "S"},
    { value: 1e24, symbol: "Sept" },
    { value: 1e27, symbol: "O" },
    { value: 1e30, symbol: "N" },
    { value: 1e33, symbol: "D" },
    { value: 1e100, symbol: "Googol" },
  ];
  const rx = /.0+$|(.[0-9]*[1-9])0+$/;
  var item = lookup.slice().reverse().find(function(item) {
    return num >= item.value;
  });
  return item ? (num / item.value).toFixed(digits).replace(rx, "$1") + item.symbol : "0";
}

</script>

<article>
  <body bgcolor="#000000">
  <header>
    <div class="grid">
      {#each upgrades as upgrade}
        <button
          class="upgrade"
          on:click={() => {
            if (clicks >= upgrade.cost) {
              if (upgrade.multiplier) {
                multiplier = multiplier * upgrade.multiplier;
                clicks -= upgrade.cost;
                upgrade.cost = upgrade.cost*5
              }
              if (upgrade.worker && clicks) {
                worker_multiplier = worker_multiplier * 2;
                workers = [upgrade.name, ...workers];
                /* start "clicking" every 1000 ms */
                setInterval(increment, 1000);
                clicks -= upgrade.cost;
                upgrade.cost = upgrade.cost *2
              }
            } else {
              alert("Click some more first!");
            }
          }}
        >
          <span>{upgrade.name}</span>
          <span>{upgrade.cost}</span>
        </button>
      {/each}
    </div>
  </header>
  <div class="game">
    <button on:click={increment} class="clicker">
      <span class="clicks">{nFormatter( clicks,1)}</span>
      <span class="pointtext">PPC: {nFormatter(multiplier)}</span>
    </button>
  </div>
  <footer>
    <div class="panelright">
      <div>
        <span>Workers</span>
        <hr />
        <div class="shop">
          {#each workers as worker}
            <div class="Worker">{worker}</div>
          {/each}
        </div>
      </div>
      <hr />
    </div>
  </footer>
</article>

<main class="container_fluid">
  <body bgcolor="#000000">
  ></body>
</main>


<style>
  .shop {
    display: grid;
    grid-auto-flow: row; /* default */
    gap: 8px;
    grid-template-rows: repeat(3, 1fr);
    grid-template-columns: repeat(3, 1fr);
  }

.upgrade span {
  color:rgb(0, 0, 0)
}

  .upgrade {
    width: 100%;
    height: 110%;
    border: 5px solid rgb(220, 220, 220);
    background-color: rgb(220, 220, 220);
    background-size: cover;
    background-image: url("https://previews.123rf.com/images/boygointer/boygointer1508/boygointer150801140/44075190-text-upgrade-button-3d-render.jpg");
    background-position: -5px -50px;
    place-items: center;
    place-content: center;
    display: flex;
    flex-direction: column;
    font: 1em "times-new-roman";
  }

  .worker {
    width: 100%;
    height: 100%;
    border: 5px solid rgb(220, 220, 220);
    background-color: rgb(0, 0, 0);
    background-image: url("https://previews.123rf.com/images/boygointer/boygointer1508/boygointer150801140/44075190-text-upgrade-button-3d-render.jpg");
    background-size: cover;
    place-items: center;
    place-content: center;
    display: flex;
  }

  .worker {
    background-image: url("https://images-prod.dazeddigital.com/592/azure/dazed-prod/1060/8/1068776.jpg")
  }

  .game {
    height: 50vh;
    display: flex;
    flex-direction: column;
    place-items: center;
    place-content: center;
  }
  .clicker {
    clip-path: ellipse();
    display: flex;
    height: 100%;
    width: 100%;
    flex-direction: column;
    place-items: center;
    place-content: center;
    background-image: url("https://i.kym-cdn.com/photos/images/original/002/491/279/26f.gif");
    background-size: cover;
    background-position: -3px -80px;
  }

  .panelright {
    height: 100%;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .clicks {
    font-size: 100px;
  }

  .pointtext {
    color: rgb(0, 0, 0);
    font-size: 25px;
    font-weight: bold;
  }

</style>
