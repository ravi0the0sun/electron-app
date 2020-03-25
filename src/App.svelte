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

	let mediaRecoder;
	const recordedChunks = [];

	async function selectSource(source) {
		videoSelectBtn.innerText = source.name;

		const constraints = {
			audio: false,
			video: {
				mandatory: {
					chromerMediaSource: 'desktop',
					chromerMediaSourceId: source.id
				}
			}
		}
		const stream = await navigator.mediaDevices.getUserMedia(constraints);

		videoElement.srcObject = stream;
		videoElement.play();

		const options= { mimeType: 'video/webm; codecs=vp9'};
		mediaRecorder = new MediaRecoder(stream, options);

		mediaRecoder.ondataavailable = handleDataAvailable;
		mediaRecoder.onstop = handleStop;
	};

	async function handleStop(e) {
		const blob = new Blob();
	}



</script>

<main>
	<h1>Electron app</h1>

	<video></video>

	<button id="startBtn" class="button is-primary">Start</button>
	<button id="stopBtn" class="button is-warning">Stop</button>

	<hr />

	<button id="videoSelect" class="button is-text">Select Window</button>

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