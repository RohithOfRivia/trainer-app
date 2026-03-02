<script lang="ts">
    import Icon from "./Icon.svelte";

    interface Props {
        label: string;
        icon?: string;
        color?: string;
        onclick?: () => void;
        fullWidth?: boolean;
        className?: string;
    }

    let { label, icon, color = 'btn-primary', onclick, fullWidth = false, className = '' }: Props = $props();
</script>

<button class="btn {color} {className} {fullWidth ? 'w-full' : ''}" {onclick}>
    {#if icon}
        <Icon name={icon}/>
    {/if}
    {label}
</button>
