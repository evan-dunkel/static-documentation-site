<script>
    import SvelteMarkdown from 'svelte-markdown';
    // import gfmHeadingId from "marked-gfm-heading-id";
    export let article;
    console.log(article[0])
    const attributes = article[0].attributes;
    // const markdownOptions = {
    //     marked.use(marked-gfm-heading-id())
    // };
</script>
<article class="prose lg:prose-xl">
    <h1>{attributes.title}</h1>
    <!-- To-Do: Add page authors and editors -->
    <!-- To-Do: Add date modified, date created -->
    <!-- To-Do: Add on this page section -->
    <SvelteMarkdown source={attributes.content}/>
    <!-- To-Do: Add edit this page widget (links to respective Strapi page) -->
</article>
<!-- Edit button -->
<p class="mt-12">
    <a class="float-right font-light text-sm text-neutral-600" href={"http://localhost:1337/admin/content-manager/collectionType/api::article.article/" + article[0].id} target="blank">Edit<img src="/diagonal_arrow.svg" class="inline-block align-bottom opacity-50" alt=""/></a>
</p>