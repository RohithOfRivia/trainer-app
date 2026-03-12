<script lang="ts">
    import type { Snippet } from "svelte";
    import Icon from "$lib/components/Icon.svelte";

    interface Tab {
        id: string;
        label: string;
        icon?: string;
    }

    interface Props {
        tabs: Tab[];
        activeTab: string;
        className?: string;
        children?: Snippet;
    }

    let { tabs, activeTab = $bindable(), className = "", children }: Props = $props();
</script>

<div class="flex flex-col w-full {className}">
    <div role="tablist" class="tabs tabs-box bg-base-200 p-0.5 rounded-2xl w-full flex">
        {#each tabs as tab}
            <button
                role="tab"
                class="flex-1 tab flex flex-wrap items-center justify-center h-auto py-2 rounded-xl font-medium gap-1 transition-colors {activeTab === tab.id ? 'tab-active text-primary' : 'text-base-content/60'}"
                onclick={() => (activeTab = tab.id)}
            >
                {#if tab.icon}
                    <Icon name={tab.icon} className="text-base shrink-0" />
                {/if}
                <span class="text-center">{tab.label}</span>
            </button>
        {/each}
    </div>

    <div role="tabpanel" class="mt-4 bg-base-300 p-5 rounded-2xl">
        {@render children?.()}
    </div>
</div>
