<script>
	import SoundFile from "$lib/components/soundFile.svelte"
	import Modal from "$lib/components/modal.svelte";
	import { onMount } from "svelte";
	import SoundPads from "$lib/components/soundPads.svelte";
	import DrumPads from "$lib/components/drumPads.svelte";


	let media = [];
	let soundClips = [];
	let mediaRecorder;
	let isOpenModal = false;

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

</script>

<section class="bg-blue-300 pt-5 pb-5 font-sans">

	<div class="text-center">
		<p class="text-6xl">Sound-Box</p>
	</div>

	<div class="container mx-auto text-center">
		<canvas class="visualizer mx-auto pb-10" width="800px" height="60px"></canvas>
		<button class="bg-green-500 hover:bg-green-600" id="record" on:click={startRecording}>Record</button>
	</div>

	<Modal {isOpenModal} on:closeModal={stopRecording} /> 

	<div class="container mx-auto">
		<SoundFile {soundClips} on:deleteSoundClip={deleteSoundClip} on:editSoundClip={editSoundClip}/>
	</div>

	<SoundPads />

	<DrumPads />
	
</section>


<style>
	
	#record {
		border: 2px solid #000000;
		padding: 2px;
		width: 85px;
	}

	button:active {
  		transform: translateY(3px);
	}

</style>