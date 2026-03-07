<script lang="ts">
    import { fade, fly } from "svelte/transition";
    import Icon from "$lib/components/Icon.svelte";
    import ActionButton from "../ActionButton.svelte";

    interface Option {
        label: string;
        value: string | number;
    }

    interface Props {
        isOpen: boolean;
        title?: string;
        options: Option[];
        onSelect: (option: Option) => void;
    }

    let { isOpen = $bindable(false), title = "Options", options, onSelect }: Props = $props();

    function close() {
        isOpen = false;
    }

    function handleSelect(option: Option) {
        onSelect(option);
        close();
    }
</script>

{#if isOpen}
    <div class="fixed inset-0 bg-black/60 backdrop-blur-sm z-[100]" transition:fade={{ duration: 150 }} onclick={close} aria-hidden="true"></div>
    <div class="fixed inset-0 z-[101] pointer-events-none flex flex-col justify-end items-center" transition:fly={{ y: "100%", duration: 150, opacity: 1 }}>
        <div class="pointer-events-auto w-full h-auto max-h-[92vh] bg-base-200 rounded-t-3xl shadow-2xl flex flex-col overflow-hidden">
            <div class="flex items-center justify-between px-4 py-3 border-b border-base-content/10 shrink-0">
                <h3 class="font-bold text-lg text-base-content m-0">{title}</h3>
               <ActionButton color="btn-neutral" className="btn-sm btn-circle" onclick={close}>
                    <Icon name="close" className="text-xl p-0 m-0" />
                </ActionButton>
            </div>
            <div class="flex-1 overflow-y-auto w-full -webkit-overflow-scrolling-touch bg-base-200 pb-8">
                <ul class="menu w-full p-2">
                    {#each options as option}
                        <li>
                            <button class="py-4 text-base font-medium" onclick={() => handleSelect(option)}>
                                {option.label}
                            </button>
                        </li>
                    {/each}
                </ul>
            </div>
        </div>
    </div>
{/if}

<style>
.-webkit-overflow-scrolling-touch { -webkit-overflow-scrolling: touch; }
</style>
