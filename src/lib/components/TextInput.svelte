<script lang="ts">
    interface Props {
        placeholder?: string;
        label?: string;
        helperText?: string;
        style?: string;
        type?: "text" | "email" | "password" | "number"; 
        value?: string;
    }

    let { 
        placeholder = "", 
        label, 
        helperText, 
        style = "", 
        type = "text",
        value = $bindable()
    }: Props = $props();
</script>

{#if label || helperText}
    <fieldset class="fieldset">
        {#if label}
            <legend class="fieldset-legend">{label}</legend>
        {/if}
        
        <input {type} {placeholder} class="input {style}" bind:value />
        
        {#if helperText}
            <p class="fieldset-label">{helperText}</p>
        {/if}
    </fieldset>
{:else}
    <input {type} {placeholder} class="input {style}" bind:value />
{/if}
