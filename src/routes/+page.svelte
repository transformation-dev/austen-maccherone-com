<script>
	import PhotoCarousel from '$lib/components/PhotoCarousel.svelte';

	import art1 from '$lib/images/Art1.jpeg';
	import art2 from '$lib/images/Art2.jpeg';
	import art3 from '$lib/images/Art3.jpeg';
	import art4 from '$lib/images/Art4.jpeg';
	import art5 from '$lib/images/Art5.jpeg';

	let photos = [
		{ src: art1, text: 'Some really cool art' },
		{ src: art2, text: 'Even more cool art' },
		{ src: art3, text: 'The coolest art3' },
		{ src: art4, text: 'The coolest art4' },
		{ src: art5, text: 'The coolest art5' },

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
