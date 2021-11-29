<script>

    import { onMount } from "svelte";

	import Card from "$lib/components/card.svelte"

    let loops = [
        {
            name: "Loop 1",
            image: "img/loop.png",
            sound: "audio/loops/loop1.wav",
            key: "n",
            isClicked: false
        },
        {
            name: "Loop 2",
            image: "img/loop.png",
            sound: "audio/loops/loop2.wav",
            key: "m",
            isClicked: false
        },
        {
            name: "Loop 3",
            image: "img/loop.png",
            sound: "audio/loops/loop3.wav",
            key: ",",
            isClicked: false
        },
        {
            name: "Loop 4",
            image: "img/loop.png",
            sound: "audio/loops/loop4.wav",
            key: ".",
            isClicked: false
        },
        {
            name: "Loop 5",
            image: "img/loop.png",
            sound: "audio/loops/loop5.wav",
            key: "/",
            isClicked: false
        }
    ]

	onMount(() => {
		document.addEventListener('keydown', (event) => {
			playMyAudio(event.key)
		})
		document.addEventListener('keyup', (event) => {
			setTimeout(() =>{
				loops = loops.map(btn => {
					btn.isClicked = false;
					return btn;
				})
				stopMyAudio(event.key)
			}, 75)
		})
	})

	function playMyAudio(keyCode){
        const button = loops.filter(btn => btn.key == keyCode).shift();
		if(button){
			let myAudio = new Audio(button.sound);
			myAudio.play();
            myAudio.loop = true;
		}
		loops = loops.map(btn => {
			if(btn.key === keyCode) btn.isClicked = true;
			return btn;
		})
    }

    function stopMyAudio(keyCode){
		const button = loops.filter(btn => btn.key == keyCode).shift();
		if(button){
			let myAudio = new Audio(button.sound);
			myAudio.pause();
		}
	}

</script>

<section class="relataive font-sans">

	<section class="mt-5 flex flex-wrap mx-auto w-3/4 gap-5 justify-center">
		{#each loops as button}
			<Card button={button} />
		{/each}
	</section>
	
</section>