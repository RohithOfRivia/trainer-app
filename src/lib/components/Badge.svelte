<script lang="ts">
    interface Props {
        label: string;
        icon?: string;
        style?: string;
    }

    let { label, icon, style = 'badge-soft badge-accent' }: Props = $props();
</script>

<span class="badge {style} gap-1">
    {#if icon}
        <span class="material-symbols-outlined" style="font-size: 12px;">{icon}</span>
    {/if}
    {label}
</span>
