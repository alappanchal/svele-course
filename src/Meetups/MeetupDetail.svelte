<script>
    import Button from "../UI/Button.svelte";
    import meetups from "./meetups-store.js";   // store
    import { createEventDispatcher } from "svelte";
    import { onDestroy } from "svelte";
   
    export let id;  // receive an id from its parent.
    
    const dispatch = createEventDispatcher();
    let selectedMeetup = {};
    
    // component is subscribing to the store for any update.
    // subscribe method return a function which is the unsubscribe itself and can be used by component to unsubscribe once its work is finish to avoid any memoery leak.
    const unsubscribe = meetups.subscribe( items =>{
        selectedMeetup = items.find( item => item.id===id );
    });

    // onDestroy is called when the component is unloaded and helpful to stop/prevent memory leak.
    onDestroy(()=>{
        unsubscribe();
    });    

    function cancelDetail(){
        dispatch("closeDetail");
    }
</script>

<style>
    section {
    margin-top: 4rem;
    }

    .image {
    width: 100%;
    height: 25rem;
    }

    img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    }

    .image {
    background: #e7e7e7;
    }

    .content {
    text-align: center;
    width: 80%;
    margin: auto;
    }

    h1 {
    font-size: 2rem;
    font-family: 'Roboto Slab', sans-serif;
    margin: 0.5rem 0;
    }

    h2 {
    font-size: 1.25rem;
    color: #6b6b6b;
    }

    p {
    font-size: 1.5rem;
    }
</style>

<section>
    <div class="image">
        <img src="{selectedMeetup.imgURL}" alt="" />
    </div>
    <div class="content">
        <h1>{selectedMeetup.title}</h1>
        <h2>{selectedMeetup.subtitle}</h2>
        <p>{selectedMeetup.description}</p>
        <Button href="mailto:{selectedMeetup.contactEmail}">Contact</Button>
        <Button type="button" mode="outline" on:click="{cancelDetail}">Cancel</Button>
    </div>
</section>