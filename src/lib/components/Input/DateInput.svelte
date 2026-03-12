<script lang="ts">
    interface Props {
        placeholder?: string;
        style?: string;
        value?: string;
    }

    let {
        placeholder = "Select Date",
        style = "",
        value = $bindable()
    }: Props = $props();

    let inputElement: HTMLInputElement;
    let inputType = $state(value ? "date" : "text");

    function handleFocus() {
        inputType = "date";
    }

    function handleBlur() {
        if (!value) inputType = "text";
    }

    function handleIconClick(e: Event) {
        e.preventDefault();
        inputType = "date";
        setTimeout(() => {
            inputElement.focus();
            try {
                inputElement.showPicker();
            } catch (err) {}
        }, 10);
    }
</script>

<label class="input input-sm {style} flex items-center gap-2 pr-3">
    <input
        bind:this={inputElement}
        type={inputType}
        {placeholder}
        class="grow bg-transparent border-none outline-none focus:ring-0 w-full hide-native-icon"
        bind:value
        onfocus={handleFocus}
        onblur={handleBlur}
    />
    <button 
        type="button"
        class="material-symbols-outlined text-base-content/50 hover:text-base-content cursor-pointer border-none bg-transparent p-0 shrink-0" 
        style="font-size: 20px;"
        onclick={handleIconClick}
    >
        calendar_month
    </button>
</label>

<style>
    .hide-native-icon::-webkit-calendar-picker-indicator {
        display: none;
        -webkit-appearance: none;
    }
</style>
