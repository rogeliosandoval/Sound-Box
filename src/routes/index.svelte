<script>
	import Modal from "$lib/components/modal.svelte";
	import { onMount } from "svelte";

	let media = [];
	let mediaRecorder = null;
	let isOpenModal = false;

	onMount(async () => {
		const stream = await navigator.mediaDevices.getUserMedia({ audio: true });

		mediaRecorder = new MediaRecorder(stream);
		mediaRecorder.ondataavailable = (e) => media.push(e.data);
		mediaRecorder.onstop = function() {
			const audio = document.querySelector('audio');
			const blob = new Blob( media, { 'type' : 'audio/ogg; codecs=opus' });
			media = [];
			audio.src = window.URL.createObjectURL(blob);
		}
	})

	function startRecording(){
		mediaRecorder.start();
	}

	function stopRecording(){
		mediaRecorder.stop();
	}

	function openModal() {
        isOpenModal = true;
	}

    function closeModal() {
        isOpenModal = false;
    }

</script>

<section class="relative pb-12 pt-12 font-sans">

	<div class="container mx-auto text-center">
		<audio class="w-1/2 mx-auto"controls></audio>
		<br>
		<button on:click={openModal} class="bg-red-500 hover:bg-red-600" id="record" on:click={startRecording}>Record</button>
	</div>

	<Modal {isOpenModal} on:closeModal={closeModal} on:closeModal={stopRecording} />


</section>


<style>

	audio::-webkit-media-controls-panel{
		background: lightyellow;
	}
	
	#record {
		border: 2px solid #000000;
		padding: 2px;
		width: 85px;
	}

	button:active {
  		transform: translateY(3px);
	}

</style>