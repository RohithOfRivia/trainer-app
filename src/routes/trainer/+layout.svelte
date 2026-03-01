<script lang="ts">
    import Sidebar from "$lib/components/Sidebar.svelte";
</script>

<Sidebar
    defaultActive="Accounts"
    items={[
        { name: 'Dashboard', icon: 'dashboard', href: '/dashboard' },
        { name: 'Clients', icon: 'group', href: '/clients' },
        { name: 'Profile', icon: 'account_circle', href: '/profile' },
        { name: 'Settings', icon: 'settings', href: '/settings' },
    ]}
/>
