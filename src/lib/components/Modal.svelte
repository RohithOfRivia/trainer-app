<script lang="ts">
    import Icon from "$lib/components/Icon.svelte";

    let { isOpen = $bindable(false), title = "Action", children } = $props();

    let dialog = $state<HTMLDialogElement>();

    $effect(() => {
        if (isOpen) dialog?.showModal();
        else dialog?.close();
    });
</script>

<dialog bind:this={dialog} class="modal modal-bottom sm:modal-middle" onclose={() => isOpen = false}>
    <div class="modal-box flex flex-col h-[92vh] sm:h-auto max-h-[90vh] rounded-t-3xl sm:rounded-3xl p-0">
        <div class="flex items-center justify-between px-4 sm:px-6 py-3 sm:py-4 border-b border-base-content/10 shrink-0">
            <h3 class="font-bold text-lg">{title}</h3>
            <form method="dialog">
                <button><Icon name="close" className="text-xl text-base-content/60" /></button>
            </form>
        </div>
        <div class="flex-1 overflow-y-auto">
            {@render children()}
        </div>
    </div>
    <form method="dialog" class="modal-backdrop">
        <button>close</button>
    </form>
</dialog>
