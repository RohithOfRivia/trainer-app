<script lang="ts">
    import { fade, fly } from "svelte/transition";
    import Icon from "$lib/components/Icon.svelte";

    let { isOpen = $bindable(false), title = "Action", children } = $props();

    function close() {
        isOpen = false;
    }
</script>

{#if isOpen}
    <div class="fixed inset-0 bg-black/60 backdrop-blur-sm z-[100]" transition:fade={{ duration: 150 }} onclick={close} aria-hidden="true"></div>
    <div class="fixed inset-0 z-[101] pointer-events-none flex flex-col justify-end items-center" transition:fly={{ y: "100%", duration: 150, opacity: 1 }}>
        <div class="pointer-events-auto w-full h-[92vh] bg-base-200 rounded-t-3xl shadow-2xl flex flex-col overflow-hidden">
            <div class="flex items-center justify-between px-4 py-3 border-b border-base-content/10 shrink-0">
                <h3 class="font-bold text-lg text-base-content">{title}</h3>
                <button class="text-base-content/60" onclick={close} title="Close">
                    <Icon name="minimize" className="text-xl" />
                </button>
            </div>
            <div class="flex-1 overflow-y-auto w-full -webkit-overflow-scrolling-touch bg-base-200">
                {@render children()}
            </div>
        </div>
    </div>
{/if}

<style>
.-webkit-overflow-scrolling-touch { -webkit-overflow-scrolling: touch; }
</style>
