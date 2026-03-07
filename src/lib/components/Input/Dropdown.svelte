<script lang="ts">
    import Icon from '$lib/components/Icon.svelte';

    interface Option {
        label: string;
        value: string | number;
    }

    interface Props {
        id: string; 
        options: Option[];
        onSelect: (option: Option) => void;
    }

    let { id, options, onSelect }: Props = $props();

    function handleSelect(option: Option) {
        onSelect(option);
        const popoverElement = document.getElementById(id);
        
        // @ts-ignore
        if (popoverElement) popoverElement.hidePopover();
    }
</script>

<button 
    class="btn btn-ghost btn-sm btn-circle" 
    popovertarget={id} 
    style="anchor-name: --anchor-{id}"
    title="Options"
>
    <Icon name="more_vert" />
</button>

<ul 
    popover="auto" 
    id={id} 
    style="position-anchor: --anchor-{id}"
    class="dropdown menu w-52 rounded-box bg-base-300 shadow-lg mt-1 z-50"
>
    {#each options as option}
        <li>
            <button onclick={() => handleSelect(option)}>
                {option.label}
            </button>
        </li>
    {/each}
</ul>
