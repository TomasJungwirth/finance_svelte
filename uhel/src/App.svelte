<script>
	let angle = 45;
  
	$: rad = (angle * Math.PI) / 180;
	$: x = 100 + 80 * Math.cos(rad);
	$: y = 100 - 80 * Math.sin(rad);
  
	$: arcPath = angle > 0 && angle < 360 
	  ? `M 100,100 L 180,100 A 80,80 0 ${angle > 180 ? 1 : 0},0 ${x},${y} Z`
	  : "";
  </script>
  
  <h1>Příklad úhel</h1>
  <p>Zadejte úhel od 0° do 360° a zobrazí se vizuální reprezentace.</p>
  
  <label>
	Zadej úhel (°): 
	<input type="number" bind:value={angle} min="0" max="360" step="1" />
  </label>
  
  <svg width="220" height="220">
	<circle cx="100" cy="100" r="80" stroke="black" stroke-width="2" fill="none" />
  
	{#if angle === 360}
	  <circle cx="100" cy="100" r="80" fill="rgba(255, 0, 0, 0.3)" />
	{:else if angle > 0}
	  <path d={arcPath} fill="rgba(255, 0, 0, 0.3)" />
	{/if}
  
	<line x1="100" y1="100" x2="180" y2="100" stroke="red" stroke-width="2" />
	{#if angle > 0}
	  <line x1="100" y1="100" x2={x} y2={y} stroke="red" stroke-width="2" />
	{/if}
  
	<circle cx="100" cy="100" r="3" fill="black" />
  </svg>
  
  <p><strong>Úhel: {angle}°</strong></p>
  
  <style>
	h1, p, label {
	  text-align: center;
	  font-family: Arial, sans-serif;
	}
	input {
	  margin: 10px;
	  padding: 5px;
	  width: 60px;
	  text-align: center;
	  display: block;
	  margin: auto;
	}
	svg {
	  display: block;
	  margin: 20px auto;
	  border: 1px solid #ccc;
	}
  </style>
  