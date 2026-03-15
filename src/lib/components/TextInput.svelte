<script lang="ts">
    import type { Snippet } from "svelte";

    interface Props {
        placeholder?: string;
        label?: string;
        helperText?: string;
        style?: string;
        type?: "text" | "email" | "password" | "number" | "date" | "search";
        value?: string | number;
        children?: Snippet;
    }

    let { 
        placeholder = "", 
        label, 
        helperText, 
        style = "input-sm", 
        type = "text",
        value = $bindable(),
        children
    }: Props = $props();
</script>

{#if label || helperText}
    <fieldset class="fieldset">
        {#if label}
            <legend class="fieldset-legend">{label}</legend>
        {/if}
        
        {#if children}
            <label class="input {style}">
                {@render children()}
                <input {type} {placeholder} class="grow" bind:value />
            </label>
        {:else}
            <input {type} {placeholder} class="input {style}" bind:value />
        {/if}
        
        {#if helperText}
            <p class="fieldset-label">{helperText}</p>
        {/if}
    </fieldset>
{:else}
    {#if children}
        <label class="input {style}">
            {@render children()}
            <input {type} {placeholder} class="grow" bind:value />
        </label>
    {:else}
        <input {type} {placeholder} class="input {style}" bind:value />
    {/if}
{/if}
