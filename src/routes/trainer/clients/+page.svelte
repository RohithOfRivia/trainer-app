<script>
    import ClientCard from "$lib/components/TrainerClients/ClientCard.svelte";
</script>

<div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-3 gap-6">
    <ClientCard
        name="Jane Doe"
        sinceMonth="January"
        sinceYear={2024}
        plan="Strength Training"
        planIcon="fitness_center"
        badgeStyle="badge-soft badge-secondary"
        lastWorkout="2 days ago"
        totalSessions={24}
        href="/clients/jane"
    />
    <ClientCard
        name="Mark Smith"
        sinceMonth="March"
        sinceYear={2023}
        plan="Weight Loss"
        planIcon="monitor_weight"
        badgeStyle="badge-soft badge-accent"
        lastWorkout="5 days ago"
        totalSessions={41}
        href="/clients/mark"
    />
    <ClientCard
        name="Sarah Lee"
        sinceMonth="August"
        sinceYear={2024}
        plan="Mobility & Flexibility"
        planIcon="self_improvement"
        badgeStyle="badge-soft badge-warning"
        lastWorkout="Today"
        totalSessions={9}
        href="/clients/sarah"
    />
</div>
