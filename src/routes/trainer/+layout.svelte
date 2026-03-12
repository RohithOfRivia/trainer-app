<script lang="ts">
    import { afterNavigate } from "$app/navigation";
    import Sidebar from "$lib/components/Navigation/Sidebar.svelte";
    let { children } = $props();

    let mainContainer = $state<HTMLElement | undefined>(undefined);

    afterNavigate(() => {
        mainContainer?.scrollTo(0, 0);
    });
</script>

<div class="flex flex-col h-screen">
    <div class="flex flex-1 overflow-hidden">
        <Sidebar
            defaultActive="Dashboard"
            items={[
                {
                    name: "Dashboard",
                    icon: "space_dashboard",
                    href: "/trainer/dashboard",
                },
                { name: "Clients", icon: "group", href: "/trainer/clients" },
                {
                    name: "Profile",
                    icon: "account_circle",
                    href: "/trainer/profile",
                },
                {
                    name: "Settings",
                    icon: "settings",
                    href: "/trainer/settings",
                },
            ]}
        />
        <main 
            bind:this={mainContainer} 
            class="flex-1 overflow-y-auto pt-6 px-6 pb-20 md:pb-0"
        >
            {@render children()}
        </main>
    </div>
</div>
