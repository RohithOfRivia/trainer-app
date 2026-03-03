<script lang="ts">
    import AvatarPlaceholder from '$lib/components/AvatarPlaceholder.svelte';
    import Badge from '$lib/components/Badge.svelte';
    import Icon from '$lib/components/Icon.svelte';

    interface Props {
        time: string;
        clientName: string;
        sessionType: string;
        duration: string;
    }

    let { time, clientName, sessionType, duration }: Props = $props();
</script>

<div class="flex flex-col sm:flex-row sm:items-center gap-2 sm:gap-4 rounded-4xl bg-base-200 p-4 sm:border sm:border-base-content/5">
    <div class="font-bold sm:border-r border-base-content/50 sm:pr-4">
        <div class="flex items-center sm:justify-center gap-2">
            <Icon name="schedule" /> {time}
        </div>
    </div>

    <div class="flex gap-3 items-center">
        <AvatarPlaceholder name={clientName} rounded="2xl" />
        <div class="flex flex-col">
            <div class="font-medium">{clientName}</div>
            <div class="text-sm opacity-75">{sessionType}</div>
        </div>
    </div>

    <div class="flex gap-2 items-center sm:justify-end w-full sm:w-auto sm:ml-auto sm:mt-0 pt-3 sm:pt-0">
        <Badge label={duration} style="badge-outline badge-accent" icon="timer" />
        <button class="btn btn-ghost btn-sm btn-circle ml-auto sm:ml-0" title="Options">
            <Icon name="more_vert" />
        </button>
    </div>
</div>
