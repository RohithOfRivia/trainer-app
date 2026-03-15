<script lang="ts">
    interface Props {
        label?: string;
        helperText?: string;
        style?: string;
        value?: string;
    }

    let {
        label,
        helperText,
        style = "input-sm",
        value = $bindable(),
    }: Props = $props();
</script>

{#if label || helperText}
    <fieldset class="fieldset">
        {#if label}
            <legend class="fieldset-legend">{label}</legend>
        {/if}
        <input type="time" class="input {style}" bind:value />
        {#if helperText}
            <p class="fieldset-label">{helperText}</p>
        {/if}
    </fieldset>
{:else}
    <input type="time" class="input {style}" bind:value />
{/if}
