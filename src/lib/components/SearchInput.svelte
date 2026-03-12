<script lang="ts">
    import Icon from "./Icon.svelte";

    interface Props {
        placeholder?: string;
        value?: string | number;
        className?: string;
    }

    let { 
        placeholder = "Search...", 
        value = $bindable(),
        className = "",
    }: Props = $props();
</script>
<label class="input input-sm {className}">
  <Icon name="search" className="text-base-content/30 text-sm"/>
    
  <input type="search" placeholder="{placeholder}" class="grow" />
</label>
