<script>
    import { onMount } from 'svelte';
    let posts = [];

    const apiUrl = process.env.SAPPER_APP_API_URL;

    onMount(async() => {
        const postsUrl = `${apiUrl}/wp/v2/posts`;
        const response = await fetch(postsUrl);
        const json = await response.json();
        posts = json;
    })
</script>

<ul>
{#each posts as post}
    <li><a rel='prefetch' href='articles/{post.slug}'>{post.title.rendered}</a></li>
{/each}
</ul>