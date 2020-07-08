<script context="module">
    const apiUrl = process.env.SAPPER_APP_API_URL;
    export async function preload({params, query}) {
        const postsUrl = `${apiUrl}/wp/v2/posts?slug=${params.slug}`;
        const response = await this.fetch(postsUrl);
        const json = await response.json();
        return { post: json[0]}
    };
    
</script>

<script>
    export let post;
    console.log(post);
</script>

<h1>{post.title.rendered}</h1>

<div class="content">
{@html post.content.rendered}
</div>