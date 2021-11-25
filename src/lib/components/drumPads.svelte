<script>

    import { onMount } from "svelte";

	import Card from "$lib/components/card.svelte"

    let drums = [
        {
            name: "Snare 1",
            image: "img/percussion/snare.png",
            sound: "audio/percussion/snare1.wav",
            key: "1",
            isClicked: false
        },
        {
            name: "Snare 2",
            image: "img/percussion/snare.png",
            sound: "audio/percussion/snare2.wav",
            key: "2",
            isClicked: false
        },
        {
            name: "Kick 1",
            image: "img/percussion/kick.png",
            sound: "audio/percussion/kick1.wav",
            key: "0",
            isClicked: false
        },
        {
            name: "Kick 2",
            image: "img/percussion/kick.png",
            sound: "audio/percussion/kick2.wav",
            key: ".",
            isClicked: false
        },
        {
            name: "High-Hat 1",
            image: "img/percussion/hat.png",
            sound: "audio/percussion/hat1.wav",
            key: "3",
            isClicked: false
        },
        {
            name: "High-Hat 2",
            image: "img/percussion/hat.png",
            sound: "audio/percussion/hat2.wav",
            key: "4",
            isClicked: false
        },
        {
            name: "Cymbal 1",
            image: "img/percussion/cymbal.png",
            sound: "audio/percussion/cymbal1.wav",
            key: "5",
            isClicked: false
        },
        {
            name: "Cymbal 2",
            image: "img/percussion/cymbal.png",
            sound: "audio/percussion/cymbal2.wav",
            key: "6",
            isClicked: false
        },
        {
            name: "Clap 1",
            image: "img/percussion/clap.png",
            sound: "audio/percussion/clap1.wav",
            key: "7",
            isClicked: false
        },
        {
            name: "Clap 2",
            image: "img/percussion/clap.png",
            sound: "audio/percussion/clap2.wav",
            key: "8",
            isClicked: false
        }
    ]

	onMount(() => {
		document.addEventListener('keydown', (event) => {
			playMyAudio(event.key)
		})
		document.addEventListener('keyup', (event) => {
			setTimeout(() =>{
				drums = drums.map(btn => {
					btn.isClicked = false;
					return btn;
				})
				stopMyAudio(event.key)
			}, 75)
		})
	})

	function playMyAudio(keyCode){
        const button = drums.filter(btn => btn.key == keyCode).shift();
		if(button){
			let myAudio = new Audio(button.sound);
			myAudio.play();
		}
		drums = drums.map(btn => {
			if(btn.key === keyCode) btn.isClicked = true;
			return btn;
		})
    }

    function stopMyAudio(keyCode){
		const button = drums.filter(btn => btn.key == keyCode).shift();
		if(button){
			let myAudio = new Audio(button.sound);
			myAudio.pause();
		}
	}

</script>

<section class="relataive font-sans">

	<section class="mt-5 flex flex-wrap mx-auto w-3/4 gap-5 justify-center">
		{#each drums as button}
			<Card button={button} />
		{/each}
	</section>
	
</section>

<style>

    /* button:active {
  		transform: translateY(5px);
	} */

</style>