<script lang="ts">
	import { onMount } from 'svelte';
	import type { PageData } from './$types';
	import autoRender from 'katex/dist/contrib/auto-render.mjs';
	import tocbot from 'tocbot';

	export let data: PageData;

	let docContainer: HTMLElement;

	onMount(() => {
		const asciidoctor = new Asciidoctor();
		docContainer.innerHTML = asciidoctor.convert(data.body, {
			attributes: { showtitle: true }
		}) as string;
		autoRender(docContainer, {
			throwOnError: false
		});
		// build the table of contents by finding all the headings
		tocbot.init({
			tocSelector: '#toc',
			contentSelector: '#markdown',
			headingSelector: 'h2, h3, h4',
			orderedList: false
		});
	});
</script>

<svelte:head>
	<script
		src=" https://cdn.jsdelivr.net/npm/@asciidoctor/core@2.2.6/dist/browser/asciidoctor.min.js "
	></script>
</svelte:head>

<main class="container m-auto grid gap-4 grid-cols-[4fr,1fr]">
	<section class="prose m-auto" id="markdown" bind:this={docContainer} role="document" />
	<section class="prose" id="toc" role="contentinfo" />
</main>

<style>
	@import 'katex/dist/katex.css';
</style>
