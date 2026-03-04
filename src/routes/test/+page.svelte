<script>
    import MobileSliderPanel from "$lib/components/MobileSliderPanel.svelte";
import WorkoutEntry from "$lib/components/Trainer/WorkoutEntry.svelte";

let isLogging = $state(false);
</script>
<div><WorkoutEntry/></div>

<button class="btn btn-primary" onclick={() => isLogging = true}>Log Workout</button>

<MobileSliderPanel bind:isOpen={isLogging} title="Log Workout">
    <WorkoutEntry />
</MobileSliderPanel>
