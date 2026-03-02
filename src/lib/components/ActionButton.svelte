<script lang="ts">
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
        <span class="material-symbols-outlined">{icon}</span>
    {/if}
    {label}
</button>
