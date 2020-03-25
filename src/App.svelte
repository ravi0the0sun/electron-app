<script>
	import desktopCapture from 'index.js';
	const videoElement = document.querySelector('video');
	const startBtn = document.getElementById('startBtn');
	const stopBtn = document.getElementById('stopBtn');
	const videoSelectBtn = document.getElementById('videoSelect');

	async function getVideoSources() {
		const inputSources = await desktopCapture.getSources({
			types: ['window', 'screen']
		});
		
		const videoOptionMenu = Menu.builtFromTemplate(
			inputSources.map(source => {
				return {
					lable: source.name,
					click: () => selectSource(source)
				};
			})
		);
		videoOptionMenu.popup();
	};



</script>

<main>
	<h1>Electron app</h1>

	<video></video>

	<button id="startBtn" class="button is-primary"></button>
	<button id="stopBtn" class="button is-warning"></button>

	<hr />

	<button id="videoSelect" class="button is-text"></button>

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