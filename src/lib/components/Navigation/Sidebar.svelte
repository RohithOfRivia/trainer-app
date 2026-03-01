<!-- Sidebar.svelte -->
<script lang="ts">
    import DesktopMenu from './DesktopMenu.svelte';
    import MobileMenu from './MobileMenu.svelte';

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
</script>

<DesktopMenu {items} {defaultActive} />
<MobileMenu {items} {defaultActive}/>
