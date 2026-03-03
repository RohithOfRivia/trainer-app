<script lang="ts">
    interface Props {
        title: string;
        value: string | number;
        icon?: string;
        desc?: string;
        trend?: number;
    }

    let { title, value, icon, desc, trend }: Props = $props();
</script>

<div class="stat bg-base-200 rounded-3xl flex-1 border-none">
    {#if icon}
        <div class="stat-figure text-primary">
            <span class="material-symbols-outlined text-4xl">{icon}</span>
        </div>
    {/if}

    <div class="stat-title">{title}</div>
    <div class="stat-value">{value}</div>

    {#if trend !== undefined}
        <div class="stat-desc flex items-center gap-1 {trend >= 0 ? 'text-success' : 'text-error'}">
            <span class="material-symbols-outlined text-sm">
                {trend >= 0 ? 'trending_up' : 'trending_down'}
            </span>
            {Math.abs(trend)}% from last month
        </div>
    {:else if desc}
        <div class="stat-desc">{desc}</div>
    {/if}
</div>
