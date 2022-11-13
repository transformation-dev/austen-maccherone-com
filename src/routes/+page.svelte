<script>
	import PhotoCarousel from '$lib/components/PhotoCarousel.svelte';

	import art1 from '$lib/images/AArt1.jpg';
	import art2 from '$lib/images/AArt2.jpg';
	import art3 from '$lib/images/A_Art3.jpg';
	import art4 from '$lib/images/AArt4.jpg';
	import art5 from '$lib/images/AArt5.jpg';
	import art6 from '$lib/images/AArt6_.jpg';
	import art7 from '$lib/images/AArt7.jpg';
	

	let photos = [
		{ src: art1, text: 'Flowers (ink)' },
		{ src: art2, text: 'Real Life Perspective (stained with coffee)' },
		{ src: art3, text: 'Still Life (charcoal on grey canson paper)' },
		{ src: art4, text: 'Color Wheel (acryllic on bristol) '},
		{ src: art5, text: '"Fire and Water" (watercolor and acryllic)' },
		{ src: art6, text: '"The Composer" (sharpie on bristol)' },
		{ src: art7, text: 'Lady With a Hat (charcoal on tan canson paper)' },
		

	];
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Austen Maccherone's website" />
</svelte:head>

<section>
	<PhotoCarousel {photos} />
</section>

<h1 id="description">Hello </h1>
<body> My passion for creation and innovation is what drives me to work and the results never fail to shock and amaze me. I love nature, it is something that is fabricated within all of us. There is a power in the intuition carved into the mind. My opinionated personality and exaggerated nature are what bring my ideas to life. 
</body>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	#description {
		text-align: center;
	}
</style>
