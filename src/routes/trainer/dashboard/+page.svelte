<script>
    import ActionButton from "$lib/components/ActionButton.svelte";
    import MetricCard from "$lib/components/MetricCard.svelte";
    import Modal from "$lib/components/Modal.svelte";
    import ScheduleCard from "$lib/components/Trainer/Dashboard/ScheduleCard.svelte";
    import WorkoutEntry from "$lib/components/Trainer/WorkoutEntry.svelte";

    let isLogging = $state(false);
    let workout = $state({
        clientName: "",
        date: new Date().toISOString().split("T")[0],
        startTime: new Date().toTimeString().slice(0, 5),
        endTime: "",
        notes: "",
        exercises: [{ id: 1, name: "", sets: [{ id: Date.now(), weight: "", reps: "" }] }],
    });

    let sessions = [
        { time: "7:00 AM", clientName: "Client Name", sessionType: "Strength Training", duration: "60min" },
        { time: "8:00 AM", clientName: "Client Name", sessionType: "S&C Training", duration: "20min" },
        { time: "8:00 AM", clientName: "Client Name", sessionType: "S&C Training", duration: "20min" },
        { time: "8:00 AM", clientName: "Client Name", sessionType: "S&C Training", duration: "20min" },
        { time: "8:00 AM", clientName: "Client Name", sessionType: "S&C Training", duration: "20min" },
        { time: "8:00 AM", clientName: "Client Name", sessionType: "S&C Training", duration: "20min" },
        { time: "8:00 AM", clientName: "Client Name", sessionType: "S&C Training", duration: "20min" },
        { time: "8:00 AM", clientName: "Client Name", sessionType: "S&C Training", duration: "20min" }
        
    ];  
    
</script>

<div class="flex flex-col gap-2">
    <h1><span class="font-medium">3</span> Sessions today</h1>

    <div class="flex flex-col md:flex-row flex-wrap gap-6 pb-5">
        <MetricCard
            title="Total Clients"
            value="1,200"
            icon="group"
            trend={4.5}
        />
        <MetricCard
            title="Sessions Today"
            value="4"
            icon="fitness_center"
            desc=""
        />
        <MetricCard title="This Week" value="20" icon="event" trend={-1.2} />
        <MetricCard
            title="Revenue"
            value="$1234"
            icon="payments"
            trend={-1.2}
        />
    </div>

    <div class="flex flex-col gap-3">
        <div class="flex justify-between items-center">
            <h3>Today's Schedule</h3>
            <ActionButton label="Add Session" icon="add" color="btn-accent" className="rounded-2xl shadow-none" onclick={() => isLogging = true} />
        </div>

        <div class="flex flex-col gap-3">
        {#each sessions as session, index}
            <ScheduleCard 
                id={index.toString()}
                time={session.time} 
                clientName={session.clientName} 
                sessionType={session.sessionType} 
                duration={session.duration}
            />
        {/each}
    </div>
    </div>
</div>


<Modal bind:isOpen={isLogging} title="Log Workout">
    <WorkoutEntry bind:workout />
</Modal>
