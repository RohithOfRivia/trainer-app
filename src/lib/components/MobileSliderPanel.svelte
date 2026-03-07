<script lang="ts">
    import { fade, fly } from "svelte/transition";
    import Icon from "$lib/components/Icon.svelte";

    let { isOpen = $bindable(false), title = "Action", children } = $props();

    const isMobile = () => window.innerWidth < 640;

    function panelTransition(node: Element) {
        return isMobile()
            ? fly(node, { y: '100%', duration: 150, opacity: 1 })
            : fade(node, { duration: 150 });
    }

    function close() {
        isOpen = false;
    }
</script>

{#if isOpen}
<div class="fixed inset-0 bg-black/60 backdrop-blur-sm z-[100]" transition:fade={{ duration: 150 }} onclick={close} aria-hidden="true"></div>
<div
    class="fixed inset-0 z-[101] pointer-events-none flex flex-col justify-end sm:justify-center items-center"
    transition:panelTransition
>
    <div class="pointer-events-auto w-full sm:w-[600px] sm:max-h-[90vh] h-[92vh] sm:h-auto bg-base-200 sm:rounded-3xl rounded-t-3xl shadow-2xl flex flex-col overflow-hidden">
        <div class="flex items-center justify-between px-4 sm:px-6 py-3 sm:py-4 border-b border-base-content/10 shrink-0">
            <h3 class="font-bold text-lg text-base-content p-0 m-0">{title}</h3>
            <button class="text-base-content/60" onclick={close} title="Close">
                <Icon name="minimize" className="text-xl" />
            </button>
        </div>
        <div class="flex-1 overflow-y-auto w-full relative -webkit-overflow-scrolling-touch bg-base-200">
            {@render children()}
        </div>
    </div>
</div>
{/if}

<style>
.-webkit-overflow-scrolling-touch { -webkit-overflow-scrolling: touch; }
</style>
