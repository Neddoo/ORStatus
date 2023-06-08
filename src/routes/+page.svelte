<script>
  import MdReplay from 'svelte-icons/md/MdReplay.svelte'
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
	<div class="flex flex-row justify-between">
		<div class="flex flex-row">
			<div class="flex-col space-y-2 mb-5 my-auto lg:ml-2">
				<h1 class="font-black text-3xl">Origin Realms Status Tracker</h1>
				<p class="text-lg opacity-50">
					Made by <a
						class="underline hover:opacity-75 duration-200"
						href="https://github.com/Neddoo">Neddo</a
					>
				</p>
			</div>
		</div>
    <div>
      <button class="w-10 h-10 mt-3" on:click={() => {location.reload();}}><MdReplay /></button>
    </div>
	</div>
	{#await fetchInfo}
		<p>Fetching data...</p>
	{:then data}
		<div class="flex-col flex lg:grid grid-cols-3 lg:space-x-2 space-y-2 lg:space-y-0">
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
						<div class="flex flex-col sm:flex-row space-y-2 sm:space-y-0 justify-between">
							<p id="serverIP">play.originrealms.com</p>
							<button
								class="border-neutral-600 border-2 my-auto text-base rounded-lg px-3"
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
