<ul>
    {#each posts as post}
        <li><a rel='prefetch' href='articles/{post.slug}'>{post.title.rendered}</a></li>
    {/each}
</ul>

<ul class="pagination">
    {#each pages as page}
        <li><button class="{ page === currentPage ? 'active' : '' }" on:click="{() => changePage(page)}">{page}</button></li>
    {/each}
</ul>

<script>
    import { onMount } from 'svelte';

    let posts = [];
    let posts_per_page = 3;
    let totalPages = null;
    let currentPage = 1;
    let pages = [];

    function createPagesArray(total) {
        let array = [];
        for(let i = 1; i <= total; i++){
            array.push(i);
        } 
        return array;
    }

    function changePage(page) {
        const postsUrl = `${apiUrl}/wp/v2/posts?per_page=${posts_per_page}&page=${page}`;
        fetch(postsUrl).then(response => {
            return response.json();
        }).then(result => {
            posts = result;
            currentPage = page; 
        });
    } 

    const apiUrl = process.env.SAPPER_APP_API_URL;

    onMount(async() => {
        const postsUrl = `${apiUrl}/wp/v2/posts?per_page=${posts_per_page}`;
        const response = await fetch(postsUrl);
        posts = await response.json();

        let totalPosts = response.headers.get('X-WP-Total');
        console.log(`totalPosts=${totalPosts}`);
        totalPages = response.headers.get('X-WP-TotalPages');
        console.log(totalPages);
        pages = createPagesArray(totalPages);     
        console.log(pages);
        
    })
</script>

<style lang="scss">
    .pagination {
        margin: 0;
        display: flex;
        justify-content: flex-start;
        flex-wrap: wrap;
        li {
            padding-right: 1em;
            padding-bottom: 1em;
            display: flex;
            align-items: center;
            
            button.active {
                background: dodgerblue;
                border-color: dodgerblue; 
            }
        }
    }
</style>