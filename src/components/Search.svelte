<script>
    import Fuse from 'fuse.js'
    import { search } from './stores.js';
    import { tick } from 'svelte';


    export let articles;
    const options = {
        isCaseSensitive: false,
        minMatchCharLength: 2,
        threshold: 0.4,
        includeMatches: true,
    keys: [
        "attributes.title",
        "attributes.content"
    ]};

    let searchTerm = '';
    const fuse = new Fuse(articles, options);
    let results = []
    let currentResultURL = "";

    function doSearch() {
        results = fuse.search(searchTerm);
        if (results.length > 0) {
            currentResultURL = "../" + results[0].item.attributes.slug;
        }
    }

    async function handleKey(event) {
        let keyCode = event.keyCode

        // exit if watched keys not present
        if (keyCode !== 38 && keyCode !== 40 && keyCode !== 75 && keyCode !== 27) return

        // handle esc key
        if (keyCode == 27) {
            search.set(false)
        } else if (keyCode == 75 && event.metaKey | event.ctrlKey) {

            // handle ⌘ + K
            search.set(!$search)


            if ($search) {
                await tick();
                document.getElementById("searchInput").focus()
            } else {
                document.getElementById("searchInput").blur()
            }
            return
        } else if ($search) {
            // handle arrow keys
            const items =  [...document.getElementsByClassName('search-area')]
            const current = document.activeElement
            const currentIndex = items.indexOf(current)
            let newIndex
            
            if (currentIndex === -1) {
                    newIndex = 0 
                } else {
                    if (keyCode === 38) newIndex = (currentIndex + items.length - 1) % items.length
                    else newIndex = (currentIndex + 1) % items.length
                }

            current.blur()
            items[newIndex].focus()
        } return
    }
</script>
<div class={"fixed top-0 w-screen h-screen bg-neutral-500/40 " + ($search ? "fixed" : "hidden")}>
    <div class="relative"></div>
<div class="relative w-screen h-screen sm:p-[10vh]">
    <div class="mx-auto overflow-hidden sm:max-w-xl sm:max-h-screen bg-neutral-200 max-h-2/3 z-2 rounded-xl">
        <form id="searchForm" action={currentResultURL}>
            <input id="searchInput" class="w-full h-16 pl-4 text-lg font-semibold border-none outline-none rounded-t-xl rounded-b-md search-area" type="search" bind:value={searchTerm} on:input={doSearch} autocomplete="off" autocorrect="off" autocapitalize="off" enterkeyhint="go" spellcheck="false" placeholder="Search docs"/>
        </form>
        <!-- Results -->
        <div class="pt-4">
                {#each results as result, i}
                <a href={"../" + result.item.attributes.slug} class="block py-2 pl-2 mx-3 mb-4 bg-white rounded-lg outline-none ring-blue-500 focus:ring hover:bg-blue-50 group search-area">
                    <div>
                        <span class="block text-sm font-bold uppercase lg:text-md text-neutral-500">{result.item.attributes.category.data.attributes.name}</span>
                        <span class="text-lg font-medium text-blue-700 group-hover:text-blue-800 lg:text-lg">{result.item.attributes.title}</span>
                    </div>
                </a>            
                {/each}
        </div>
        <div class="px-2 py-1 pb-2 align-middle bg-neutral-100">
            <p class="font-light sm"><span class="text-xs p-0.5 rounded font-semibold border-2 border-color-neutral-700">Enter</span> to select</p>
        </div>
    </div>
</div>
</div>
<svelte:window on:keydown={handleKey} />
