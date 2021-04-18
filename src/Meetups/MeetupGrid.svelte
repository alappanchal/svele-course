<script>
    import MeetupItem from "./MeetupItem.svelte";
    import FilteredMeetup from "./FilteredMeetup.svelte";
    import Button from "../UI/Button.svelte";
    import { createEventDispatcher } from "svelte";
    import { scale } from "svelte/transition";
    import { flip } from "svelte/animate";
    export let meetups;
    
    const dispatch = createEventDispatcher();
    
    let favOnly = false;    
    function filterMeetups(event){
        favOnly = event.detail === 1;
    }

    $: filteredMeetupsList = ( favOnly ) ?  meetups.filter( item => item.isFavorite ) : meetups;
</script>

<style>
    #meetups {
        width: 100%;
        display: grid;
        grid-template-columns: 1fr;
        grid-gap: 1rem;
    }

    #meetup-controls{
        margin: 1rem;
        display: flex;
        justify-content: space-between;

    }

    @media (min-width: 768px) {
        #meetups {
            grid-template-columns: repeat(2, 1fr);
        }
    }

    .no-message{
        margin: 1rem;
    }

</style>

<section id="meetup-controls">
    <FilteredMeetup on:filterby="{filterMeetups}"/>
    <Button on:click="{ () => dispatch('add') }" >New Meetup</Button>
</section>

<section id="meetups">  
    {#if filteredMeetupsList.length===0}
        <p class="no-message">The Server has no meetups. Please start by adding one.</p>
    {/if}
    <!-- 
        on:toggleFavorite below listend for custom event 'toggleFavorite'
    -->  
    {#each filteredMeetupsList as meetup (meetup.id) }        
        <div transition:scale="{{duration: 300}}" animate:flip>
            <MeetupItem 
                id={meetup.id}
                title={meetup.title}
                subtitle={meetup.subtitle}
                imgURL={meetup.imgURL}
                description={meetup.description}
                address={meetup.address}
                email={meetup.contactEmail}
                isFav={meetup.isFavorite}
                on:showDetail
                on:edit                       
            />
        </div>
    {/each}
</section>
