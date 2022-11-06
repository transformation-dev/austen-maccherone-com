<script>
	import PhotoCarousel from '$lib/components/PhotoCarousel.svelte';

	import art1 from '$lib/images/Art1.jpeg';
	import art2 from '$lib/images/Art2.jpg';
	import art3 from '$lib/images/Art3.jpeg';
	import art4 from '$lib/images/Art4.jpg';
	import art5 from '$lib/images/Art5.jpg';
	import art6 from '$lib/images/Art6.jpg';
	import art7 from '$lib/images/Art7.jpeg';
	import art8 from '$lib/images/Art8.jpeg';

	let photos = [
		{ src: art1, text: 'Ensure we are on the Graphics page' },
		{ src: art2, text: 'Even more cool art' },
		{ src: art3, text: 'Still Life (charcoal on grey canson paper)' },
		{ src: art4, text: 'Color Wheel (acryllic on bristol) '},
		{ src: art5, text: '"Ducky in a Pond" (multimedia)' },
		{ src: art6, text: 'Real Life Perspective (stained with coffee)' },
		{ src: art7, text: 'Mask Graphic' },
		{ src: art8, text: 'Flowers (ink)' },

	];
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Austen Maccherone's website" />
</svelte:head>

<section>
	<PhotoCarousel {photos} />
</section>

<h1 id="description">Hello</h1>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	#description {
		text-align: left;
	}
</style>
