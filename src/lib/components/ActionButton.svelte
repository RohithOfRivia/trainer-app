<script lang="ts">
    import type { Snippet } from "svelte";
    import Icon from "./Icon.svelte";

    interface Props {
        label?: string;
        icon?: string;
        color?: string;
        onclick?: () => void;
        fullWidth?: boolean;
        className?: string;
        children?: Snippet;
    }

    let { label, icon, color = 'btn-primary', onclick, fullWidth = false, className = '', children }: Props = $props();
</script>

<button class="btn {color} {className} {fullWidth ? 'w-full' : ''}" {onclick}>
    {#if icon}
        <Icon name={icon}/>
    {/if}
    {#if label}{label}{/if}
    {#if children}
        {@render children()}
    {/if}
</button>
