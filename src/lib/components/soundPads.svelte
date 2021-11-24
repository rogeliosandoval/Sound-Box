<script>

	import Card from "$lib/components/card.svelte"

	const buttons = [
		{
			name: "Train",
			image: "img/train.png",
			sound: "audio/train.wav",
            key: "1"
		},
		{
			name: "Golfer",
			image: "img/golf.png",
			sound: "audio/golf.wav",
            key: "2"
		},
		{
			name: "Bell",
			image: "img/bell.png",
			sound: "audio/bell.wav",
            key: "3"
		},
		{
			name: "Gun",
			image: "img/gun.png",
			sound: "audio/gun.wav",
            key: "4"
		},
		{
			name: "Toilet",
			image: "img/toilet.png",
			sound: "audio/toilet.wav",
            key: "5"
		}
	]

</script>

<section class="relataive font-sans pt-5 bg-blue-200">

	<section class="mt-10 flex flex-wrap mx-auto w-3/4 gap-5 justify-center">
		{#each buttons as button}
			<Card button={button} />
		{/each}
	</section>
	
</section>