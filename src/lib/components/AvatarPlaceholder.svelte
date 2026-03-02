<script lang="ts">
    interface Props {
        name: string;
        size?: string;
        rounded?: string;
        className?: string;
    }

    let { 
        name, 
        size = 'w-12',
        rounded = "full",
        className = ""
    }: Props = $props();

    // Safely grab the first letter and capitalize it, fallback to '?'
    let initial = $derived(name ? name.charAt(0).toUpperCase() : '?');
</script>

<div class="avatar placeholder {className}">
    <div class="bg-neutral text-neutral-content rounded-{rounded} {size} flex items-center justify-center">
        <span class="text-lg">{initial}</span>
    </div>
</div>
