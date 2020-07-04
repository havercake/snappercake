<script>
    import { onMount } from 'svelte';
    let posts = [];
    onMount(async() => {
        const postsUrl = 'https://sapperpress.rockpaper.technology/wp-json/wp/v2/posts';
        const response = await fetch(postsUrl);
        const json = await response.json();
        posts = json;
    })
</script>

<ul>
{#each posts as post}
    <li>{post.title.rendered}</li>
{/each}
</ul>