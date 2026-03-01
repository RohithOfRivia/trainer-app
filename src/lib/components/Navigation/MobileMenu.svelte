<script lang="ts">
    interface MenuItem {
        name: string;
        icon: string;
        href: string;
    }

    interface Props {
        items: MenuItem[];
        defaultActive?: string;
    }

    let { items, defaultActive = '' }: Props = $props();
    let activeItem = $state(defaultActive);

    function onMenuItemClick(name: string, href: string) {
        activeItem = name;
        window.location.href = href;
    }
</script>

<div class="dock dock-md md:hidden">
    {#each items as menuItem}
        <button
            class={activeItem === menuItem.name ? 'dock-active' : ''}
            onclick={() => onMenuItemClick(menuItem.name, menuItem.href)}
        >
            <span class="material-symbols-outlined">{menuItem.icon}</span>
            <span class="dock-label">{menuItem.name}</span>
        </button>
    {/each}
</div>
