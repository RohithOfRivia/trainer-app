<script lang="ts">
    import { page } from "$app/state";
    import Icon from "../Icon.svelte";

    interface MenuItem {
        name: string;
        icon: string;
        href: string;
    }

    interface Props {
        items: MenuItem[];
        defaultActive?: string;
    }

    let { items, defaultActive = "" }: Props = $props();

    let navbarOpen = $state(false);

    function onNavbarClick() {
        navbarOpen = !navbarOpen;
    }
</script>

<aside
    class="hidden md:flex flex-col items-center h-screen sticky top-0 overflow-y-auto space-y-4 py-6 px-2 bg-base-300
    transition-[width] duration-200
    {navbarOpen ? 'w-56' : 'w-14'}"
>
    <ul class="w-full flex flex-col">
        <li>
            <button
                class="flex items-center p-2 rounded-full hover:bg-base-content/10 gap-2"
                onclick={onNavbarClick}
            >
                <Icon name="menu" />
            </button>
        </li>
        {#each items as menuItem}
            <li>
                <a
                    href={menuItem.href}
                    class="flex items-center p-2 rounded-full gap-5 w-full
                        {page.url.pathname === menuItem.href
                        ? 'bg-base-content/20'
                        : 'hover:bg-base-content/10'}"
                >
                    <span class="material-symbols-outlined"
                        >{menuItem.icon}</span
                    >

                    {#if navbarOpen}
                        <div>{menuItem.name}</div>
                    {/if}
                </a>
            </li>
        {/each}
    </ul>
</aside>
