<script>
    import Button from "../UI/Button.svelte";
    import Badge from "../UI/Badge.svelte";    
    import meetups from "./meetups-store";   // store
    import { createEventDispatcher }  from "svelte";    

    export let id;
    export let title;
    export let subtitle;
    export let imgURL;
    export let description;
    export let address;    
    export let isFav;

    let isFavoriteInProgress = false;

    function toggleFavorite(){
      isFavoriteInProgress = true;
      fetch('https://svelte-course-20c5e-default-rtdb.firebaseio.com/meetups/' + id +'.json', {
          method: 'PATCH',    // syntax for firebase to update existing record.
          body: JSON.stringify({isFavorite: !isFav}),
          headers: { 'Content-Type':"application/json" }
      }).then( res =>{
          if ( !res.ok ){
              throw new Error("HTTP Update Favorite Meetup Error");
          }
          isFavoriteInProgress = false;
          meetups.updateFavorite(id);    // Local store update
      }).catch( err =>{
        isFavoriteInProgress = false;
          console.log (err)
      });      
    }

    const dispatch = createEventDispatcher();
    function showDetail(){
      dispatch("showDetail",id);
    }
</script>

<style>
    article {
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
      border-radius: 5px;
      background: white;
      margin: 1rem;
    }
  
    header,
    .content,
    footer {
      padding: 1rem;
    }
  
    .image {
      width: 100%;
      height: 14rem;
    }
  
    .image img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  
    h1 {
      font-size: 1.25rem;
      margin: 0.5rem 0;
      font-family: "Roboto Slab", sans-serif;
    }
  
    
  
    h2 {
      font-size: 1rem;
      color: #808080;
      margin: 0.5rem 0;
    }
  
    p {
      font-size: 1.25rem;
      margin: 0;
    }
  
    div {
      text-align: right;
    }

    .content{
      height: 4rem;;
    }
  </style>

<article id="{id}">
    <header>
        <h1>
          {title}
          {#if isFav}
            <Badge>FAVORITE</Badge>
          {/if}
        </h1>
        <h2>{subtitle}</h2>
        <p>{address}</p>
    </header>
    <div class="image">
        <img src="{imgURL}" alt="{title}" />
    </div>
    <div class="content">
        <p>{description}</p>        
    </div>
    <footer>
        <Button mode="outline" type="button" on:click="{ () => dispatch('edit',id) }">Edit</Button>        
        <!-- 
            on:click event handler triggers/dispatch the custom event 'toggerlFavorite' along with 'id' value
        -->
        <Button 
          mode="outline" 
          color="{isFav ? null : 'success'}" 
          type="button" 
          loadingIcon="{isFavoriteInProgress}"
          on:click="{toggleFavorite}" >
          {isFav ? 'Unfavorite' : 'Favorite'}
        </Button>
        <Button type="button" on:click="{showDetail}">Show Detail</Button>
    </footer>
</article>