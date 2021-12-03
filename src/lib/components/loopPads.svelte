<script>

    import { onMount } from "svelte";

	import LoopCards from "$lib/components/cards/loopCards.svelte"

    export let loops = [
        {
            name: "Loop",
            image: "img/loop.png",
            sound: "audio/loops/loop1.wav",
            key: "n",
            isClicked: false
        },
        {
            name: "Loop",
            image: "img/loop.png",
            sound: "audio/loops/loop2.wav",
            key: "m",
            isClicked: false
        },
        {
            name: "Loop",
            image: "img/loop.png",
            sound: "audio/loops/loop3.wav",
            key: ",",
            isClicked: false
        },
        {
            name: "Loop",
            image: "img/loop.png",
            sound: "audio/loops/loop4.wav",
            key: ".",
            isClicked: false
        },
        {
            name: "Loop",
            image: "img/loop.png",
            sound: "audio/loops/loop5.wav",
            key: "/",
            isClicked: false
        }
    ]

    export let myAudio;

	onMount(() => {
		document.addEventListener('keydown', (event) => {
            if(event.key === 'Escape'){
                resetLoops()
            } else {
                playMyAudio(event.key)
            }

		})
	})

    function resetLoops(){
        myAudio.pause();
        myAudio.currentTime = 0;
        loops = loops.map(btn => {
            btn.isClicked = false;
            return btn;
        })
    }

	function playMyAudio(keyCode){
        if(keyCode !== ' '){
            const button = loops.filter(btn => btn.key == keyCode).shift();
            if(!button.isClicked){
                myAudio = new Audio(button.sound);
                myAudio.play();
                myAudio.loop = true;
            } else {
                myAudio.pause();
                myAudio.currentTime = 0;
            }
            loops = loops.map(btn => {
                if(btn.key === keyCode && !btn.isClicked) btn.isClicked = true;
                else btn.isClicked = false;
                return btn;
            })
        }
    }

</script>

<section class="relataive font-sans">

	<section class="mt-5 flex flex-wrap mx-auto w-3/4 gap-5 justify-center">
		{#each loops as button}
			<LoopCards button={button} />
		{/each}
	</section>
	
</section>