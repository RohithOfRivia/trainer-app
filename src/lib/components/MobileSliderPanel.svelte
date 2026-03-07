<script lang="ts">
    import { fade, fly } from "svelte/transition";
    import Icon from "$lib/components/Icon.svelte";

    let { isOpen = $bindable(false), title = "Action", children } = $props();

    /* ─── State-Driven Drag Gesture ─── */
    let currentY = $state(0);
    let isDragging = $state(false);
    let startY = 0;

    // Reset position instantly whenever the modal is opened
    $effect(() => {
        if (isOpen) {
            currentY = 0;
            isDragging = false;
        }
    });

    function onPointerDown(e: PointerEvent) {
        isDragging = true;
        startY = e.clientY - currentY;
        (e.currentTarget as HTMLElement).setPointerCapture(e.pointerId);
    }

    function onPointerMove(e: PointerEvent) {
        if (!isDragging) return;
        currentY = Math.max(0, e.clientY - startY);
    }

    function onPointerUp(e: PointerEvent) {
        if (!isDragging) return;
        isDragging = false;
        
        if (currentY > 150) {
            // Closes modal. Notice we DO NOT reset currentY here. 
            // It stays at >150px so it doesn't jerk upwards!
            isOpen = false; 
        } else {
            // Didn't drag far enough, snap smoothly back to 0
            currentY = 0;
        }
    }

    function close() {
        currentY = 0;
        isOpen = false;
    }
</script>

{#if isOpen}
    <div class="fixed inset-0 bg-black/60 backdrop-blur-sm z-[100]" transition:fade={{ duration: 150 }} onclick={close} aria-hidden="true"></div>
    
    <div
        class="fixed inset-0 z-[101] pointer-events-none flex flex-col justify-end sm:justify-center items-center"
        transition:fly={{ y: '100%', duration: 150, opacity: 1 }}
    >
        <div 
            class="pointer-events-auto w-full sm:w-[600px] sm:max-h-[90vh] h-[92vh] sm:h-auto bg-base-200 sm:rounded-3xl rounded-t-3xl shadow-2xl flex flex-col overflow-hidden"
            style="transform: translateY({currentY}px); transition: {isDragging ? 'none' : 'transform 0.3s cubic-bezier(0.2, 0.8, 0.2, 1)'};"
        >
            
            <div 
                class="w-full shrink-0 bg-base-200 z-10 touch-none cursor-grab active:cursor-grabbing"
                onpointerdown={onPointerDown} onpointermove={onPointerMove} onpointerup={onPointerUp} onpointercancel={onPointerUp}
            >
                <div class="w-full pt-3 pb-2 sm:hidden flex justify-center">
                    <div class="w-12 h-1.5 bg-base-content/20 rounded-full"></div>
                </div>
                <div class="flex items-center justify-between px-4 sm:px-6 pb-3 sm:py-4 border-b border-base-content/10">
                    <h3 class="font-bold text-lg text-base-content">{title}</h3>
                    <div class="flex items-center gap-1 cursor-auto" onpointerdown={(e) => e.stopPropagation()}>
                        <button class="btn btn-sm btn-circle btn-ghost text-base-content/60 hover:text-base-content hover:bg-base-content/10 border-none" onclick={close} title="Close">
                            <Icon name="keyboard_arrow_down" className="text-xl sm:hidden" />
                            <Icon name="close" className="text-xl hidden sm:block" />
                        </button>
                    </div>
                </div>
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
