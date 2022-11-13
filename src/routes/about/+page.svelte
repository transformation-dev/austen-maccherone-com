<svelte:head>
	<title>About</title>
	<meta name="description" content="About this website" />
</svelte:head>

<script>
	import photoMe from '$lib/images/Me.jpg';
</script>

<div class="text-column">
	<h1>About Me</h1>
	<img src={photoMe} alt="Austen Maccherone" />
	

	<h1>About this website</h1>

	<p>
		This website was created using the
		<a href="https://kit.svelte.dev">SvelteKit starter template </a>
		using the following command line and following the prompts:
	</p>

	<pre>npm create svelte@latest</pre>

	<p>
		Then I built a carousel component by getting inspiration from
		<a href="https://github.com/E01T/simple-svelte-slideshow">Simple Svelte Slideshow</a>
		component
	</p>
</div>
