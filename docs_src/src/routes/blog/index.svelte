<script context="module">
	export async function preload() {
		const posts = await this.fetch(`api/blog/get`).then(r => r.json()).catch(e => console.log(e));;
		return { posts };
	}
</script>

<script>
	export let posts;
</script>

<svelte:head>
	<title>Blog • Svelte Native</title>
</svelte:head>

<div class='posts stretch'>
	{#each posts as post}
		<article class='post' data-pubdate={post.metadata.dateString}>
			<a class="no-underline" rel='prefetch' href='blog/{post.slug}' title='Read the article »'>
				<h2>{post.metadata.title}</h2>
				<p>{post.metadata.description}</p>
			</a>
		</article>
	{/each}
</div>

<style>
	.posts {
		grid-template-columns: 1fr 1fr;
		grid-gap: 1em;
		min-height: calc(100vh - var(--nav-h));
		padding: var(--top-offset) var(--side-nav) 6rem var(--side-nav);
		max-width: var(--main-width);
		margin: 0 auto;
	}
	h2 {
		display: inline-block;
		margin: 3.2rem 0 0.4rem 0;
		color: var(--text);
		max-width: 18em;
		font-size: var(--h3);
		font-weight: 400;
	}
	.post:first-child {
		margin: 0 0 2rem 0;
		padding: 0 0 4rem 0;
		border-bottom: var(--border-w) solid #6767785b; /* based on --second */
	}
	.post:first-child h2 {
		font-size: 4rem;
		font-weight: 400;
		color: var(--second);
	}
	.post:first-child::before,
	.post:nth-child(2)::before {
		content: 'Latest post • ' attr(data-pubdate);
		color: var(--flash);
		font-size: var(--h6);
		font-weight: 400;
		letter-spacing: .05em;
		text-transform: uppercase;
	}
	.post:nth-child(2)::before {
		content: 'Older posts';
	}
	.post p {
		font-size: var(--h5);
		max-width: 30em;
		color: var(--second);
	}
	.post > a { display: block }
	.posts a:hover,
	.posts a:hover > h2 {
		color: var(--flash)
	}
</style>