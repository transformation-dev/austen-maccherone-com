<script>
	import welcome from '$lib/images/svelte-welcome.webp';
	import welcome_fallback from '$lib/images/svelte-welcome.png';
	import SliderGallery from '$lib/components/SliderGallery.svelte';

	import magpie from '$lib/images/magpie.jpg';
	import flick from '$lib/images/flick.jpg';
	import blackbird from '$lib/images/blackbird.jpg';

	let imgArray = [
		{ src: magpie, text: 'Magpie' },
		{ src: flick, text: 'Flick' },
		{ src: blackbird, text: 'Blackbird' }
	];
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Austen Maccherone's website" />
</svelte:head>

<section>
	<h1>Welcome</h1>

	<SliderGallery {imgArray} />
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}
</style>
