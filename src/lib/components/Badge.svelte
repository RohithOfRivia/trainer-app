<script lang="ts">
    interface Props {
        label: string;
        icon?: string;
        style?: string;
    }

    let { label, icon, style = 'badge-soft badge-accent' }: Props = $props();
</script>

<span class="badge {style} gap-1 h-auto py-1 text-wrap text-center">
    {#if icon}
        <span class="material-symbols-outlined shrink-0" style="font-size: 12px;">{icon}</span>
    {/if}
    <span class="">{label}</span>
</span>
