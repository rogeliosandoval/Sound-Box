<link rel="stylesheet" href="https://demos.creative-tim.com/notus-js/assets/styles/tailwind.css">
<link rel="stylesheet" href="https://demos.creative-tim.com/notus-js/assets/vendor/@fortawesome/fontawesome-free/css/all.min.css">

<script>

	import Card from "$lib/components/card.svelte"

	const buttons = [
		{
			name: "Train",
			image: "../../img/train.png",
			sound: "audio/train.wav"
		},
		{
			name: "Golfer",
			image: "../../img/golf.png",
			sound: "audio/golf.wav"
		},
		{
			name: "Bell",
			image: "../../img/bell.png",
			sound: "audio/bell.wav"
		},
		{
			name: "Gun",
			image: "../../img/gun.png",
			sound: "audio/gun.wav"
		},
		{
			name: "Toilet",
			image: "../../img/toilet.png",
			audio: "audio/toilet.wav"
		}
	]

</script>

<section class="relataive font-sans pt-5 bg-blue-200">

	<div class="text-center">
		<p class="text-6xl">Sound-Box</p>
	</div>

	<section class="mt-10 flex flex-wrap mx-auto w-3/4 gap-5 justify-center">
		{#each buttons as button}
			<Card button={button} />
		{/each}
	</section>
	
</section>