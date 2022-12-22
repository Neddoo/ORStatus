<script>
	const fetchInfo = (async () => {
		const response = await fetch('https://mc-api.grphcrtv.com/v1/ping/play.originrealms.com');
		return await response.json();
	})();

  // @ts-ignore

	const copyContent = async () => {
		try {
      // @ts-ignore
			await navigator.clipboard.writeText(document.getElementById('serverIP').innerHTML);
			console.log('Content copied to clipboard');
			// @ts-ignore
			document.getElementById('copybtn').innerHTML = 'Copied!';
			setTimeout(() => {
				// @ts-ignore
				document.getElementById('copybtn').innerHTML = 'Copy';
			}, 2000);
		} catch (err) {
			console.error('Failed to copy: ', err);
		}
	};
</script>

<main class="p-5">
	<div class="flex-col space-y-2 mb-5">
		<h1 class="font-black text-3xl">Origin Realms Status Tracker</h1>
		<p class="text-lg opacity-50">
			Made by <a class="underline hover:opacity-75 duration-200" href="https://github.com/Neddoo"
				>Neddo</a
			>
		</p>
	</div>
	{#await fetchInfo}
		<p>Fetching data...</p>
	{:then data}
		<div class="grid grid-cols-3 space-x-2">
			<div class="border-neutral-700 border-2 rounded-lg">
				<div class="p-5 flex flex-col space-y-2">
					<h3 class="text-xl font-bold">Online Players</h3>
					<div class="text-xl opacity-75">
						<div class="flex space-x-1">
							<p>{data.players.online}</p>
							<span>/</span>
							<p class="opacity-50">{data.players.max}</p>
						</div>
					</div>
				</div>
			</div>
			<div class="border-neutral-700 border-2 rounded-lg">
				<div class="p-5 flex flex-col space-y-2">
					<h3 class="text-xl font-bold">IP</h3>
					<div class="text-xl opacity-75">
						<div class="flex space-x-1 justify-between">
							<p id="serverIP">{data.hostname}</p>
							<button
								class="border-neutral-600 border-2 rounded-lg px-3"
								id="copybtn"
								on:click={copyContent}>Copy!</button
							>
						</div>
					</div>
				</div>
			</div>
			<div class="border-neutral-700 border-2 rounded-lg">
				<div class="p-5 flex flex-col space-y-2">
					<h3 class="text-xl font-bold">Version</h3>
					<div class="text-xl opacity-75">
						<div class="flex space-x-1">
							<p>{data.version.name}</p>
						</div>
					</div>
				</div>
			</div>
		</div>
	{:catch error}
		<p>An error occured!</p>
	{/await}
</main>
