
<script>
    export let controlType = null;
    export let id;
    export let label;
    export let value;
    export let type = "text";    
    export let rows = null;       
    export let valid = true;
    export let validatyMessage = '';

    let touched = false;
</script>

<style>
    input,
    textarea {
        display: block;
        width: 100%;
        font: inherit;
        border: none;
        border-bottom: 2px solid #ccc;
        border-radius: 3px 3px 0 0;
        background: white;
        padding: 0.15rem 0.25rem;
        transition: border-color 0.1s ease-out;
    }

    input:focus,
    textarea:focus {
        border-color: #e40763;
        outline: none;
    }

    label {
        display: block;
        margin-bottom: 0.5rem;
        width: 100%;
    }

    .form-control {
        padding: 0.5rem 0;
        width: 100%;
        margin: 0.25rem 0;
    }

    .invalid{
        border: 1px solid #f00;
        background-color: #fde3e3;
    }

    .error-message{
        color: #f00;
        margin: 0.25rem 0;        
    }
</style>

<div class="form-control">
    <label for="{id}">{label}</label>
    {#if controlType === "textarea"}
        <textarea class:invalid="{!valid && touched}" row="{rows}" id="{id}" value="{value}" on:input on:blur="{ () => touched = true }" />
    {:else}
        <input class:invalid="{!valid && touched}" type="{type}" id="{id}" value="{value}" on:input on:blur="{ () => touched = true }" />
    {/if}
    
    {#if !valid && validatyMessage && touched }
        <p class="error-message">{validatyMessage}</p>
    {/if}
</div>