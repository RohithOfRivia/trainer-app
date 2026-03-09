<script lang="ts">
    interface Props {
        label: string;
    }

    let { label }: Props = $props();

    let style = $derived.by(() => {
        const type = label.toLowerCase();
        if (type === 'breakfast') return 'badge-soft badge-warning';
        if (type === 'lunch') return 'badge-soft badge-accent';
        if (type === 'dinner') return 'badge-soft badge-info';
        return 'badge-soft';
    });
</script>

<span class="badge {style} w-24 gap-1 h-auto py-1 text-center">
    <span class="truncate">{label}</span>
</span>
