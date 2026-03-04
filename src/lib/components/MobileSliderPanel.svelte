<script lang="ts">
    import { fade, fly, scale } from "svelte/transition";
    import Icon from "$lib/components/Icon.svelte";

    let { 
        isOpen = $bindable(false), 
        title = "Action", 
        children 
    } = $props();

    let isMinimized = $state(false);

    /* ─── Sheet Drag Gesture (Mobile) ─── */
    let sheetDragY = $state(0);
    let isDraggingSheet = $state(false);
    let sheetStartY = 0;

    function onSheetPointerDown(e: PointerEvent) {
        isDraggingSheet = true;
        sheetStartY = e.clientY;
        e.currentTarget?.setPointerCapture(e.pointerId);
    }

    function onSheetPointerMove(e: PointerEvent) {
        if (!isDraggingSheet) return;
        // Only allow dragging downwards
        sheetDragY = Math.max(0, e.clientY - sheetStartY);
    }

    function onSheetPointerUp(e: PointerEvent) {
        if (!isDraggingSheet) return;
        isDraggingSheet = false;
        
        // If dragged down more than 100px, minimize it
        if (sheetDragY > 100) {
            isMinimized = true;
        }
        // Reset transform so it snaps back or stays hidden cleanly
        sheetDragY = 0; 
    }

    /* ─── Bubble Drag Gesture (Minimized State) ─── */
    let bubbleX = $state(0);
    let bubbleY = $state(0);
    let isDraggingBubble = $state(false);
    let hasDraggedBubble = $state(false);
    let startMouseX = 0;
    let startMouseY = 0;
    let startBubbleX = 0;
    let startBubbleY = 0;

    function onBubblePointerDown(e: PointerEvent) {
        isDraggingBubble = true;
        hasDraggedBubble = false;
        startMouseX = e.clientX;
        startMouseY = e.clientY;
        startBubbleX = bubbleX;
        startBubbleY = bubbleY;
        e.currentTarget?.setPointerCapture(e.pointerId);
    }

    function onBubblePointerMove(e: PointerEvent) {
        if (!isDraggingBubble) return;
        const dx = e.clientX - startMouseX;
        const dy = e.clientY - startMouseY;
        
        // If moved more than 5px, register as a drag instead of a click
        if (Math.abs(dx) > 5 || Math.abs(dy) > 5) hasDraggedBubble = true;
        
        bubbleX = startBubbleX + dx;
        bubbleY = startBubbleY + dy;
    }

    function onBubblePointerUp(e: PointerEvent) {
        if (!isDraggingBubble) return;
        isDraggingBubble = false;
        
        // If it was just a tap (no dragging), maximize the modal
        if (!hasDraggedBubble) {
            isMinimized = false;
        }
    }

    function close() {
        isOpen = false;
        setTimeout(() => {
            isMinimized = false;
            bubbleX = 0;
            bubbleY = 0;
        }, 300);
    }
</script>

{#if isOpen && !isMinimized}
    <div 
        class="fixed inset-0 bg-black/60 backdrop-blur-sm z-[100]" 
        transition:fade={{ duration: 200 }} 
        onclick={close}
        aria-hidden="true"
    ></div>
{/if}

{#if isOpen && !isMinimized}
    <div
        class="fixed bottom-0 left-0 right-0 sm:left-1/2 sm:-translate-x-1/2 sm:top-1/2 sm:-translate-y-1/2 sm:bottom-auto w-full sm:w-[600px] sm:max-h-[90vh] h-[92vh] sm:h-auto bg-base-200 sm:rounded-3xl rounded-t-3xl shadow-2xl z-[101] flex flex-col overflow-hidden"
        style="
            transform: translateY({sheetDragY}px) {sheetDragY === 0 ? '' : 'translateZ(0)'}; 
            transition: {isDraggingSheet ? 'none' : 'transform 0.3s cubic-bezier(0.2, 0.8, 0.2, 1)'};
        "
        transition:fly={{ y: '100%', duration: 300, opacity: 1 }}
    >
        <div 
            class="w-full pt-3 pb-2 cursor-grab active:cursor-grabbing touch-none sm:hidden flex justify-center shrink-0 bg-base-200 relative z-10"
            onpointerdown={onSheetPointerDown}
            onpointermove={onSheetPointerMove}
            onpointerup={onSheetPointerUp}
            onpointercancel={onSheetPointerUp}
        >
            <div class="w-12 h-1.5 bg-base-content/20 rounded-full"></div>
        </div>

        <div class="flex items-center justify-between px-4 sm:px-6 pb-3 sm:py-4 border-b border-base-content/10 shrink-0 bg-base-200">
            <h3 class="font-bold text-lg text-base-content">{title}</h3>
            <div class="flex items-center gap-1">
                <button class="btn btn-sm btn-circle btn-ghost text-base-content/60 hover:text-base-content hover:bg-base-content/10 border-none" onclick={() => isMinimized = true} title="Minimize">
                    <Icon name="keyboard_arrow_down" className="text-xl" />
                </button>
                <button class="btn btn-sm btn-circle btn-ghost text-base-content/60 hover:text-base-content hover:bg-base-content/10 border-none" onclick={close} title="Close">
                    <Icon name="close" className="text-xl" />
                </button>
            </div>
        </div>

        <div class="flex-1 overflow-y-auto w-full relative -webkit-overflow-scrolling-touch">
            {@render children()}
        </div>
    </div>
{/if}

{#if isOpen && isMinimized}
    <div
        class="fixed bottom-6 right-6 z-[102]"
        style="transform: translate({bubbleX}px, {bubbleY}px);"
        transition:scale={{ duration: 250, start: 0.5, opacity: 0 }}
    >
        <button
            class="w-14 h-14 bg-primary text-primary-content rounded-full shadow-lg shadow-primary/30 flex items-center justify-center cursor-grab active:cursor-grabbing touch-none transition-transform active:scale-95"
            onpointerdown={onBubblePointerDown}
            onpointermove={onBubblePointerMove}
            onpointerup={onBubblePointerUp}
            onpointercancel={onBubblePointerUp}
        >
            <Icon name="fitness_center" className="text-2xl" />
        </button>

        <button 
            class="absolute -top-1 -right-1 w-6 h-6 bg-base-300 border border-base-content/10 text-base-content rounded-full flex items-center justify-center shadow-md hover:bg-base-content/10 hover:text-error transition-colors z-10"
            onclick={(e) => { e.stopPropagation(); close(); }}
            title="Close Workout"
        >
            <Icon name="close" className="text-[14px]" />
        </button>
    </div>
{/if}

<style>
    /* Ensures smooth scrolling specifically on iOS devices */
    .-webkit-overflow-scrolling-touch {
        -webkit-overflow-scrolling: touch;
    }
</style>
