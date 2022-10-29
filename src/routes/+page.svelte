<script>
	import PhotoCarousel from '$lib/components/PhotoCarousel.svelte';

	import magpie from '$lib/images/magpie.jpg';
	import flick from '$lib/images/flick.jpg';
	import blackbird from '$lib/images/blackbird.jpg';

	let photos = [
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
	<PhotoCarousel {photos} />
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}
</style>
