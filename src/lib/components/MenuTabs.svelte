<script lang="ts">
    import type { Snippet } from "svelte";

    interface Tab {
        id: string;
        label: string;
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
    <div role="tablist" class="tabs tabs-border pb-5">
        {#each tabs as tab}
            <button
                role="tab"
                class="tab {activeTab === tab.id ? 'tab-active' : ''}"
                onclick={() => (activeTab = tab.id)}
            >
                {tab.label}
            </button>
        {/each}
    </div>

    <div role="tabpanel" class="mt-[-1px] rounded-b-box rounded-tr-box">
        {@render children?.()}
    </div>
</div>
