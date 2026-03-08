<script lang="ts">
    import type { Snippet } from "svelte";

    interface Tab {
        label: string;
        content: Snippet;
    }

    interface Props {
        tabs: Tab[];
        name?: string;
        className?: string;
    }

    let { 
        tabs, 
        name = "desktop_tabs_" + Math.random().toString(36).slice(2), 
        className = "" 
    }: Props = $props();
</script>

<div class="tabs bg-base-100 tabs-border flex {className}">
    {#each tabs as tab, i}
        <input
            type="radio"
            name={name}
            class="tab"
            aria-label={tab.label}
            checked={i === 0}
        />
        <div class="tab-content bg-base-300 p-10">
            {@render tab.content()}
        </div>
    {/each}
</div>
