<script>
    import { onMount } from 'svelte';
    import {Input} from '@smui/textfield';
    import {Icon} from '@smui/common';
    import Fab from '@smui/fab';
    import Paper from '@smui/paper'
    import DataTable, { Head, Body, Row, Cell } from '@smui/data-table';

    import axios from 'axios';
    //save all the stories in this array
    let understory = [];
    //checks if the hope contains one of these strings
    function RegexMatch(value){
        //desided to only allow the word rather than words that contain these words as got 'lewisham'
        const regexs = [/hope\b/, /wish\b/, /want\b/, /dream\b/, /vision\b/, /for\b/, /will\b/, /is to\b/];
        for(let i=0; i<regexs.length; i++){
            if(regexs[i].test(value.hope)){
                return true;
            }
        }
        return false;
    }

    onMount( async () => { 
       await axios.get('https://strapi.understory.community/gathered-hopes?_limit=30000')
       .then(function (response) {
           understory = response.data.filter(RegexMatch);
       })
       .catch(function (error) {
           console.log(error);
       })
    });

    let value = '';
    let filter = '';

    function doSearch(){
        filter = value;
        value = '';
    }

    function handleKeyDown(event) {
        if(event.key === 'Enter'){
            doSearch();
        }
    }

    function filterHopes(hope){
        let regexFilter = new RegExp(filter);
        return regexFilter.test(hope);        
    }

</script>

<main>
   <h2>People's Hopes</h2>
   <div class="solo-demo-container solo-container">
   <Paper class="solo-paper" elevation={20}>
    <Icon class="material-icons">search</Icon>
    <Input
      bind:value
      on:keydown={handleKeyDown}
      placeholder="Search"
      class="solo-input"
    />
   </Paper>
   <Fab
   on:click={doSearch}
    disabled={value === ''}
    color="primary"
    mini
    class="solo-fab"
   >
    <Icon class="material-icons">arrow_forward</Icon>
   </Fab>
   </div>
   <DataTable table$aria-label="hope list" style="width: 100%; ">
       <Head>
           <Row>
               <Cell numeric>ID</Cell>
               <Cell>Hope</Cell>
               <Cell>Created</Cell>
               <Cell>Published</Cell>
               <Cell>Updated</Cell>
           </Row>
        </Head>
        <Body>
           {#each understory as story}
               {#if filter == '' || filterHopes(story.hope)}
                   <Row>
                       <Cell numeric>{story.id}</Cell>
                       <Cell >{story.hope}</Cell>
                       <Cell>{story.created_at}</Cell>
                       <Cell>{story.published_at}</Cell>
                       <Cell>{story.updated_at}</Cell>
                   </Row>
               {/if}
           {:else}
           <p>Loading..</p>
           {/each}
       </Body>
    </DataTable>
</main>
<!--wouldnt want styling here but its just while i learn-->
<style>
  .solo-demo-container {
    padding: 36px 18px;

  }
 
  .solo-container {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  * :global(.solo-paper) {
    display: flex;
    align-items: center;
    flex-grow: 1;
    max-width: 600px;
    margin: 0 12px;
    padding: 0 12px;
    height: 48px;
  }
  * :global(.solo-paper > *) {
    display: inline-block;
    margin: 0 12px;
  }
  * :global(.solo-input) {
    flex-grow: 1;
    color: var(--mdc-theme-on-surface, #000);
  }
  * :global(.solo-input::placeholder) {
    color: var(--mdc-theme-on-surface, #000);
    opacity: 0.6;
  }
  * :global(.solo-fab) {
    flex-shrink: 0;
  }

  * :global(.mdc-data-table__cell){
      max-width: 20vw;
  }

  * :global(.mdc-data-table__header-cell--numeric), * :global(.mdc-data-table__cell--numeric) {
    text-align: left;
  }

</style>