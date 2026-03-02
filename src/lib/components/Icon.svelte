<script lang="ts">
    interface Props {
        name: string;
        className?: string;
    }

    let { name, className = "" }: Props = $props();
</script>

<span class="material-symbols-outlined {className}">
    {name}
</span>
