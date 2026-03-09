<script lang="ts">
    import { page } from "$app/state";

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
</script>

<div class="dock dock-md md:hidden z-10">
    {#each items as menuItem}
        <a
            href={menuItem.href}
            class={page.url.pathname === menuItem.href ? "dock-active" : ""}
        >
            <span class="material-symbols-outlined">{menuItem.icon}</span>
            <span class="dock-label">{menuItem.name}</span>
        </a>
    {/each}
</div>
