<script>
    import MobileSliderPanel from "$lib/components/MobileSliderPanel.svelte";
    import Modal from "$lib/components/Modal.svelte";
    import WorkoutEntry from "$lib/components/Trainer/WorkoutEntry.svelte";

    let isLogging = $state(false);
    
    // Shared state that persists across open/close
    let workout = $state({
        clientName: "",
        date: new Date().toISOString().split("T")[0],
        notes: "",
        exercises: [{ id: 1, name: "", sets: 3, reps: 10, weight: 0 }],
    });
</script>

<!-- <div><WorkoutEntry bind:workout /></div> -->

<button class="btn btn-primary" onclick={() => isLogging = true}>Log Workout</button>

<Modal bind:isOpen={isLogging} title="Log Workout">
    <WorkoutEntry bind:workout />
</Modal>

