<script lang="ts">
    import AvatarPlaceholder from "$lib/components/AvatarPlaceholder.svelte";
import ActionButton from "../../ActionButton.svelte";
    import Badge from "../../Badge.svelte";


    interface Props {
        name: string;
        sinceMonth: string;
        sinceYear: string | number;
        plan: string;
        planIcon?: string;
        badgeStyle?: string;
        lastWorkout: string;
        totalSessions: number;
        href?: string;
    }

    let { name, sinceMonth, sinceYear, plan, planIcon, badgeStyle, lastWorkout, totalSessions, href = '#' }: Props = $props();
</script>

<div class="card bg-base-200 rounded-2xl p-4 flex flex-col gap-4 w-full">

    <!-- Row 1: Avatar + Name + Since -->
    <div class="flex items-center gap-3">
        <AvatarPlaceholder name="aaaaa"/>
        <div class="flex flex-col">
            <span class="font-semibold">{name}</span>
            <span class="text-xs text-base-content/50">Sinasdsadce {sinceMonth} {sinceYear}</span>
        </div>
    </div>

    <!-- Row 2: Plan badge -->
    <div>
        <Badge label={plan} icon={planIcon} style={badgeStyle} />
    </div>

    <!-- Row 3: Last workout + Total sessions -->
    <div class="flex justify-between text-sm">
        <div class="flex flex-col">
            <span class="text-base-content/50 text-xs">Last Session</span>
            <span>{lastWorkout}</span>
        </div>
        <div class="flex flex-col items-end">
            <span class="text-base-content/50 text-xs">Total Sessions</span>
            <span>{totalSessions}</span>
        </div>
    </div>

    <!-- Row 4: View client button -->
    <ActionButton label="View Client" icon="person" color="btn-soft btn-accent btn-sm" fullWidth={true} onclick={() => window.location.href = href} />
        
</div>
