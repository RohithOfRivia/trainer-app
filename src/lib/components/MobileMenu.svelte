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

    let navbarOpen = $state(false);
    let activeItem = $state(defaultActive);

    function onNavbarClick() {
        navbarOpen = !navbarOpen;
    }

    function onMenuItemClick(name: string, href: string) {
        activeItem = name;
        navbarOpen = false;
        window.location.href = href;
    }
</script>

<div class="md:hidden">
    <button
        class="fixed top-4 left-4 z-50 flex items-center p-2 rounded-full bg-base-300 hover:bg-base-content/10 md:hidden"
        onclick={onNavbarClick}
    >
        <span class="material-symbols-outlined">menu</span>
    </button>

    <button
        class="fixed inset-0 z-40 bg-black/40 transition-opacity duration-200 cursor-default
            {navbarOpen ? 'opacity-100 pointer-events-auto' : 'opacity-0 pointer-events-none'}"
        onclick={onNavbarClick}
        aria-label="Close menu"
    >
    </button>
    
    <aside class="fixed top-0 left-0 h-full w-64 z-50 flex flex-col bg-base-300 py-6 px-4 transition-transform duration-200
        {navbarOpen ? 'translate-x-0' : '-translate-x-full'}">
            
        <span class="p-2 border-b border-base-content/10">TrainerApp</span>

        <ul class="w-full flex flex-col">
            {#each items as menuItem}
                <li>
                    <button
                        class="flex items-center p-3 rounded-full gap-5 w-full transition-colors duration-200
                            {activeItem === menuItem.name ? 'bg-base-content/20' : 'hover:bg-base-content/10'}"
                        onclick={() => onMenuItemClick(menuItem.name, menuItem.href)}
                    >
                        <span class="material-symbols-outlined">{menuItem.icon}</span>
                        <div>{menuItem.name}</div>
                    </button>
                </li>
            {/each}
        </ul>
    </aside>
</div>
