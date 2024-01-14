<script>
    export let pages;
    export let categories;
    import { search } from './stores.js';
</script>
<div class="sticky top-0 w-64 h-full max-h-screen py-10 pl-6 pr-3 overflow-x-hidden overflow-y-auto xl:w-80 2xl:w-96 xl:pr-5 2xl:pr-6">

    <!-- Title  -->
    <a href="/">
        <span class="font-bold lg:text-3xl">Netsys</span>
    </a>
    <div class="h-8"></div>

    <!-- Search -->
    <button type="button" aria-label="Search" on:click={search.set(!$search)}>
        <span class="relative py-2 pl-4 pr-20 text-lg font-normal rounded-full text-neutral-500 bg-neutral-200 hover:ring ring-inset ring-neutral-300">
            <span>Search</span>
            <div class="absolute right-0 inline pr-4 text-neutral-400">
                <span class="font-bold border-2 rounded border-neutral-300">⌘</span>
                <span class="rounded border-2 border-neutral-300 ml-[-1px] font-medium px-[1px]">K</span>
            </div>
        </span>
    </button>

    <!-- Menu Items -->
    <div class="h-8"></div>
    {#each categories as category}
    {#if category.attributes.name != "Hidden"}
    <ul class="pb-4">
        <li class="list-none">
            <span class="block pb-4 font-black uppercase lg:text-md text-neutral-700">{category.attributes.name}</span>
            <ul>
                {#each pages as page}
                    <!-- To-Do: Clean up, the data object for page category is many layers within. -->
                    {#if page.attributes.category.data.attributes.name == category.attributes.name}
                        <li>
                            <!-- To-Do: Highlight and scroll to current page in menu -->
                            <!-- <a class={"block lg:text-md pl-4 pb-1 font-normal hover:text-blue-400 transition-all " + page.slug == currentPage.slug ? "font-semibold invert hover:text-black" : ""} href={page.url}>{page.title}</a> -->
                            <a class={"block lg:text-md pl-4 pb-1 font-normal hover:text-blue-400 duration-150 transition-all"} href={'../' + page.attributes.slug}>{page.attributes.title}</a>
                        </li>
                    {/if}
                {/each}
            </ul>
        </li>
    </ul>
    {/if}
    {/each}
</div>