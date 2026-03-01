<script>
    import MetricCard from "$lib/components/MetricCard.svelte";


</script>
<div class="flex flex-col md:flex-row flex-wrap gap-6">
    <MetricCard title="Total Clients" value="1,200" icon="group" trend={4.5} />
    <MetricCard title="Sessions Today" value="4" icon="fitness_center" desc="" />
    <MetricCard title="This Week" value="20" icon="event" trend={-1.2} />
    <MetricCard title="Revenue" value="$1234" icon="payments" trend={-1.2} />
</div>
