<script lang="ts">
    import AvatarPlaceholder from '$lib/components/AvatarPlaceholder.svelte';
    import Badge from '$lib/components/Badge.svelte';
    import Icon from '$lib/components/Icon.svelte';
    import Dropdown from '$lib/components/Input/Dropdown.svelte';
    import MobileOptionsPanel from '$lib/components/Input/MobileOptionsPanel.svelte';

    interface Props {
        id: string;
        time: string;
        clientName: string;
        sessionType: string;
        duration: string;
    }

    let { id, time, clientName, sessionType, duration }: Props = $props();

    const dummyOptions = [
        { label: 'Edit Session', value: 'edit' },
        { label: 'Reschedule', value: 'reschedule' },
        { label: 'Cancel', value: 'cancel' }
    ];

    let isMobileMenuOpen = $state(false);

    function handleOptionSelect(option: { label: string; value: string | number }) {
        console.log(option.value);
    }
</script>

<div class="relative flex flex-col sm:flex-row sm:items-center gap-2 sm:gap-4 rounded-4xl bg-base-200 p-4 sm:border sm:border-base-content/5 hover:bg-base-200/50 transition-colors">
    
    <a href="/trainer/dashboard/1" class="absolute inset-0 z-0 rounded-4xl">
        <span class="sr-only">View Session Details</span>
    </a>

    <div class="font-bold sm:border-r border-base-content/50 sm:pr-4 pr-10">
        <div class="flex items-center sm:justify-center gap-2 text-base-content/70">
            <Icon name="schedule"/>{time}
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
        
        <div class="hidden sm:block sm:ml-0 relative z-10">
            <Dropdown 
                id={id} 
                options={dummyOptions} 
                onSelect={handleOptionSelect}
            />
        </div>

        <div class="absolute top-4 right-4 sm:hidden z-1">
            <button class="btn btn-ghost btn-sm btn-circle" onclick={() => isMobileMenuOpen = true} title="Options">
                <Icon name="more_vert" />
            </button>
        </div>
    </div>
</div>

<MobileOptionsPanel 
    bind:isOpen={isMobileMenuOpen} 
    title="Session Options" 
    options={dummyOptions}
    onSelect={handleOptionSelect}
/>
