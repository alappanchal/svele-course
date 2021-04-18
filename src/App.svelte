<script>
    
    import MeetupGrid from "./Meetups/MeetupGrid.svelte";        
    import EditMeetup from "./Meetups/EditMeetup.svelte";
    import meetups from "./Meetups/meetups-store.js";   // store
    import MeetupDetail from "./Meetups/MeetupDetail.svelte";
    import Header from "./UI/Header.svelte";
    import LoadingSpinner from "./UI/LoadingSpinner.svelte";
    import Error from "./UI/Error.svelte";
    import { onMount  } from "svelte";

    let editMode = null;    
    let page = 'overview';
    let id = ''; 
    let editID = null;  
    let isLoading = false;
    let httpError;

    function savedMeetup(){ 
        page="overview";
        editMode = null;        
        editID = null;
    }    

    function cancelEdit(){
        editMode = null;
        editID = null;
    }

    function showDetail(event){
        page = "detail";
        id = event.detail;        
    }

    function closeDetail(){
        page = 'overview';
        id = "";        
    }

    function editMeetup(event){
        editMode = "edit";
        editID = event.detail;
    }

    onMount( () => {
        isLoading = true;
        setTimeout(fetchMeetupData,1000);
    });

    function fetchMeetupData(){
		fetch('https://svelte-course-20c5e-default-rtdb.firebaseio.com/meetups.json')
		.then(res => {		
            isLoading = false;
			if (!res.ok){
				throw new Error("Fetch Meetup Data failed!");
			}
			// parse the json to javascript object and also returns the promise	to the next then clase
			return res.json();	
		}).then(data =>{			
			const fetchedMeetups = [];
			for (let key in data){								
                fetchedMeetups.push({ id: key,...data[key]});
			}            
            fetchedMeetups.reverse();
			meetups.setMeetups(fetchedMeetups);
		}).catch(err => {
            httpError = err;
            isLoading = false;		
			console.log ( err );
		});
	}

    function clearError(){
        httpError = null;
    }
    

</script> 

<style>
    main{
        margin-top: 5rem;
    } 
    
</style>

{#if httpError}
    <Error errorMessage="{httpError.message}" on:cancelModal="{clearError}" />
{/if}

<!-- Header Component -->
<Header />

<main>
    {#if page=='overview'}           
        {#if editMode === 'edit'}
            <EditMeetup id="{editID}" on:saveMeetup="{savedMeetup}" on:cancelModal="{cancelEdit}" />    
        {/if}      
        
        {#if isLoading}
            <LoadingSpinner message="Fetching Meetups Data"/>
        {:else}
            <MeetupGrid 
                meetups={$meetups} 
                on:showDetail="{showDetail}" 
                on:edit="{editMeetup}"
                on:add="{ () => editMode='edit' }"
            />    
        {/if}
        
    {:else}
        <!--
            1. Custom Component - MeetupDetail - imported in script block.
            2. Pass the data to 'MeetupDetail' component from app
            3. Listens for component's custom event 'closeDetail'
        -->
        <MeetupDetail {id} on:closeDetail="{closeDetail}" />
    {/if}
</main>
