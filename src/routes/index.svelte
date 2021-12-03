<script>
	import SoundFile from "$lib/components/soundFile.svelte"
	import Modal from "$lib/components/modal.svelte";
	import { onMount } from "svelte";
	import SoundPads from "$lib/components/soundPads.svelte";
	import DrumPads from "$lib/components/drumPads.svelte";
	import LoopPads from "$lib/components/loopPads.svelte";
	import GamePads from "$lib/components/gamePads.svelte";


	let media = [];
	let soundClips = [];
	let mediaRecorder;
	let isOpenModal = false;
	let showPad = true;
	let showDrum = true;
	let showGame = true;
	let showLoop = true;
	let audioFile;
	let theLoops;

	let audioCtx, canvas, canvasCtx;

	//IF YOU USE DOCUMENT IT NEEDS TO BE INSIDE ONMOUNT
	onMount(async () => {

		const mic = await navigator.mediaDevices.getUserMedia({ audio: true });
		canvas = document.querySelector('.visualizer');
		canvasCtx = canvas.getContext('2d');
		visualize(mic);

		mediaRecorder = new MediaRecorder(mic);
		mediaRecorder.ondataavailable = (e) => media.push(e.data);
		mediaRecorder.onstop = function() {
			const clipName = prompt('Enter a name for your sound clip','My Unnamed Clip');

			const blob = new Blob( media, { 'type' : 'audio/wav; codecs=opus' });
			media = [];
			const audioURL = window.URL.createObjectURL(blob);
			const newSoundClip = {
				label: (clipName === null) ? "Default" : clipName,
				srcURL: audioURL,
				id: crypto.randomUUID()
			}
			console.log(newSoundClip)
			//spread y rest
			soundClips = [...soundClips,newSoundClip];
		}

	});

	function visualize(stream) {
		if(!audioCtx) {
			audioCtx = new AudioContext();
		}

		const source = audioCtx.createMediaStreamSource(stream);

		const analyser = audioCtx.createAnalyser();
		analyser.fftsize = 2048;
		const bufferLength = analyser.frequencyBinCount;
		const dataArray = new Uint8Array(bufferLength);

		source.connect(analyser);

		draw()

		function draw() {
			const WIDTH = canvas.width;
			const HEIGHT = canvas.height;

			requestAnimationFrame(draw);

			analyser.getByteTimeDomainData(dataArray);

			canvasCtx.fillStyle = 'rgb(200, 200, 200)';
			canvasCtx.fillRect(0, 0, WIDTH, HEIGHT);

			canvasCtx.lineWidth = 2;
			canvasCtx.strokeStyle = 'rgb(0, 0, 0)';

			canvasCtx.beginPath();

			let sliceWidth = WIDTH * 1.0 / bufferLength;
			let x = 0;

			for (let i = 0; i < bufferLength; i++) {
				let v = dataArray[i] / 128.0;
				let y = v * HEIGHT/2;

				if(i === 0) {
					canvasCtx.moveTo(x, y);
				} else {
					canvasCtx.lineTo(x, y);
				}

				x += sliceWidth;
			}

			canvasCtx.lineTo(canvas.width, canvas.height/2);
			canvasCtx.stroke();
		}
	}

	function startRecording(){
		isOpenModal = true;
		mediaRecorder.start();
	}

	function stopRecording(){
		isOpenModal = false;
		mediaRecorder.stop();
	}

	function deleteSoundClip(event){
		soundClips = soundClips.filter(soundClip => soundClip.id != event.detail)
	}

	function editSoundClip(event){
		const passedInId = event.detail;
		console.log(passedInId)
		const newClipName = prompt("Enter new clip name:");
		soundClips = soundClips.map(soundClip => {
			if(soundClip.id == passedInId){
				soundClip.label = (newClipName === null) ? soundClip.label : newClipName;
			}
			return soundClip;
		})
	}

	// function resetSoundBoard(){
	// 	audioFile.pause();
	// 	audioFile.currentTime = 0;
	// 	theLoops = theLoops.map(btn => {
	// 		btn.isClicked = false;
	// 		return btn;
	// 	})
	// }

</script>

<section class="pt-5 pb-5 font-sans">

	<div class="fixed">
		<img class="title" alt="SoundBox" src="img/general/soundbox.png">

		<div class="sideInfo mx-10">
			<p class="neonText mx-auto">BG by <a class="text-green-500" href="https://www.artstation.com/ivxn">Ivan Alvarez</a></p>
			<br>
			<label class="text-white text-xl font-bold text tracking-wider">
				<input type="checkbox" bind:checked={showPad}> Misc Sounds
			</label>
			<br>
			<label class="text-white text-xl font-bold text tracking-wider neonTextStay">
				<input type="checkbox" bind:checked={showDrum}> Percussion
			</label>
			<br>
			<label class="text-white text-xl font-bold text tracking-wider">
				<input type="checkbox" bind:checked={showGame}> Game Sounds
			</label>
			<br>
			<label class="text-white text-xl font-bold text tracking-wider neonTextStay">
				<input type="checkbox" bind:checked={showLoop}> Loops
			</label>
		</div>
	</div>

	<div class="container mx-auto text-center">
		<canvas class="visualizer mx-auto pb-10" height="60px"></canvas>
		<button class="bg-green-500 hover:bg-green-600" id="record" on:click={startRecording}>Record</button>
	</div>

	<Modal {isOpenModal} on:closeModal={stopRecording} /> 

	<div class="container mx-auto">
		<SoundFile {soundClips} on:deleteSoundClip={deleteSoundClip} on:editSoundClip={editSoundClip}/>
	</div>

	{#if showPad}
		<SoundPads />
	{/if}

	{#if showDrum}
		<DrumPads />
	{/if}

	{#if showGame}
		<GamePads />
	{/if}

	{#if showLoop}
		<LoopPads />
		<div class="mx-auto pt-5">
			<div class="w-60 mx-auto text-center border-black border-4 bg-red-50 text-lg">
				press <b>ESC</b> to stop loops
			</div>
		</div>
	{/if}


	<!-- <div class="text-center pt-5">
		<button on:click={resetSoundBoard} id="stopSound" class="bg-red-500 hover:bg-red-600 font-bold">STOP AUDIO</button>
	</div> -->
	
</section>


<style>

	@media screen and (max-width:1280px) {
		.title {
			width: 250px;
			height: 155px;
		}
	}

	@media screen and (max-width:640px) {
		.title {
			display: none;
		}

		.sideInfo {
			display: none;
		}
	}

	.neonTextStay{
		text-shadow:
			0 0 40px #0fa,
			0 0 80px #0fa,
			0 0 90px #0fa,
			0 0 100px #0fa,
			0 0 150px #0fa;
	}

	.neonText {
		font-size: 20px;
		font-weight: bold;
		animation: flicker 1.5s infinite alternate;
  		color: #fff;
	}

	@keyframes flicker {
		0%, 18%, 22%, 25%, 53%, 57%, 100% {
	
			text-shadow:
			0 0 4px #fff,
			0 0 11px #fff,
			0 0 19px #fff,
			0 0 40px #0fa,
			0 0 80px #0fa,
			0 0 90px #0fa,
			0 0 100px #0fa,
			0 0 150px #0fa;
		
		}
		
		20%, 24%, 55% {        
			text-shadow: none;
		}    
  	}
	
	#record {
		border: 2px solid #000000;
		padding: 2px;
		width: 90px;
	}

	/* #stopSound {
		border: 2px solid #000000;
		padding: px;
		width: 120px;
		height: 50px;
	} */

	button:active {
  		transform: translateY(3px);
	}

</style>