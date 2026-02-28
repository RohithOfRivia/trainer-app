<script>
    let isExpanded = $state(true);
    let { children } = $props();

    function toggleSidebar() {
        isExpanded = !isExpanded;
    }
</script>

<div class="drawer lg:drawer-open">
    <input id="sidebar-drawer" type="checkbox" class="drawer-toggle" />

    <div class="drawer-content flex flex-col">
        <div class="navbar lg:hidden border-b border-accent">
            <label for="sidebar-drawer" class="btn btn-ghost">
                <span class="material-symbols-outlined">menu</span>
            </label>
        </div>

        <main class="p-6">
            {@render children()}
        </main>
    </div>

    <div class="drawer-side z-20">
        <label for="sidebar-drawer" class="drawer-overlay"></label>
        
        <aside class="flex flex-col bg-base-200 h-full duration-300 w-80 {isExpanded ? 'lg:w-64' : 'lg:w-20'}">
            
            <div class="hidden lg:flex items-center h-16 px-4">
                <button onclick={toggleSidebar} class="btn btn-ghost btn-circle">
                    <span class="material-symbols-outlined">menu</span>
                </button>
                {#if isExpanded}
                    <span class="ml-4 font-bold text-xl">Trainer</span>
                {/if}
            </div>

            <div class="flex lg:hidden items-center h-16 px-4">
                <span class="font-bold text-xl">Trainer App</span>
            </div>

            <ul class="menu flex-1 px-2 gap-2">
                <li>
                    <a href="/trainer/dashboard" class="flex items-center gap-4">
                        <span class="material-symbols-outlined text-2xl">dashboard</span>
                        <span class="{!isExpanded ? 'lg:hidden' : ''}">
                            Dashboard
                        </span>
                    </a>
                </li>
            </ul>

            <div class="p-4 border-t border-accent">
                <div class="flex items-center gap-4">
                    <span class="material-symbols-outlined text-2xl">account_circle</span>
                    <span class="font-bold {!isExpanded ? 'lg:hidden' : ''}">
                        Rohith
                    </span>
                </div>
            </div>
        </aside>
    </div>
</div>
