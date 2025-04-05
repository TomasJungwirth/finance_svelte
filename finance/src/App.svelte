<script>
	import { writable } from 'svelte/store';
  
	let transakce = writable([]);
	let zustatek = writable(0);
  
	let nazevTransakce = '';
	let castka = 0;
	let typTransakce = 'income';
  
	function pridejTransakci() {
	  if (nazevTransakce && castka) {
		transakce.update((currentTransactions) => [
		  ...currentTransactions,
		  { nazevTransakce, castka: parseFloat(castka), typTransakce, id: Date.now() },
		]);
		aktualniZustatek();
			  nazevTransakce = '';
		castka = 0;
	  }
	}
  
	function aktualniZustatek() {
	  transakce.subscribe((currentTransactions) => {
		let total = 0;
		currentTransactions.forEach((transaction) => {
		  if (transaction.typTransakce === 'income') {
			total += transaction.castka;
		  } else {
			total -= transaction.castka;
		  }
		});
		zustatek.set(total);
	  });
	}
  
	function odstranTransakci(id) {
	  transakce.update((currentTransactions) =>
		currentTransactions.filter((transaction) => transaction.id !== id)
	  );
	  aktualniZustatek();
	}
  </script>
  
  
  <h1>Příklad finance</h1>
  <p>V tomto příkladu lze zapisovat své příjmy a výdaje. Je zde zobrazen zůstatek, který se aktualizuje podle aktuálního seznamu příjmů a výdajů. Přiadané položky lze jednoduše odstranit ze seznamu pomocí křížku.</p>
  
  <div class="container">
	<h1>Osobní finance</h1>
  
	<div class="form">
	  <input type="text" bind:value={nazevTransakce} placeholder="Název transakce" />
	  <input type="number" bind:value={castka} placeholder="Částka v Kč" min="0" />
	  <select bind:value={typTransakce}>
		<option value="income">Příjem</option>
		<option value="expense">Výdaj</option>
	  </select>
	  <button on:click={pridejTransakci}>Přidat transakci</button>
	</div>
  
	<h3 style="text-align:center;">Zůstatek: { $zustatek } Kč</h3>
  
	<div class="transactions">
	  {#each $transakce as { nazevTransakce, castka, typTransakce, id }}
		<div class="transaction {typTransakce}" key={id}>
		  <div>
			<strong>{nazevTransakce}</strong>: {castka} Kč
		  </div>
		  <button class="remove-butn" on:click={() => odstranTransakci(id)}>X</button>
		</div>
	  {/each}
	</div>
  </div>
  
  
  <style>
	.container {
	  width: 90%;
	  max-width: 500px;
	  margin: 50px auto;
	  padding: 15px;
	  background: white;
	  border: 1px solid #ddd;
	  border-radius: 5px;
	  box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
	}
	h1 {
	  text-align: center;
	  color: #333;
	}
	.form {
	  display: flex;
	  flex-direction: column;
	  gap: 8px;
	}
	input, select, button {
	  padding: 8px;
	  border: 1px solid #ccc;
	  border-radius: 4px;
	  font-size: 14px;
	}
	button {
	  background-color: #4CAF50;
	  color: white;
	  font-weight: bold;
	  cursor: pointer;
	  border: none;
	  transition: 0.2s;
	}
	button:hover {
	  background-color: #45a049;
	}
	.transactions {
	  margin-top: 15px;
	}
	.transaction {
	  padding: 10px;
	  border: 1px solid #ddd;
	  border-radius: 4px;
	  display: flex;
	  justify-content: space-between;
	  align-items: center;
	  margin-bottom: 8px;
	}
	.income {
	  background-color: #dff0d8;
	}
	.expense {
	  background-color: #f8d7da;
	}
	.remove-butn {
	  background-color: #d9534f;
	  border: none;
	  color: white;
	  padding: 5px;
	  border-radius: 3px;
	  cursor: pointer;
	}
	.remove-butn:hover {
	  background-color: #c9302c;
	}
  </style>
  