<script>
    import { metatags } from '@roxi/routify';
  import Noop from '@roxi/routify/runtime/decorators/Noop.svelte';
    import axios from 'axios';
    import { onMount } from 'svelte';
    
    metatags.title = 'Freemdis : Freemius WordPress Directory';
    metatags.description = 'Find easily WordPress products running with Freemius';

    var current_page = 1;
    var page_limit   = 30;
    var next_page;
    var prev_page;
    var default_wp_version = '6.1';
    var freemius_plugins_lists = "https://api.wordpress.org/plugins/info/1.2/?action=query_plugins&request[page]="+current_page+"&request[per_page]="+page_limit+"&request[author]=freemius&request[wp_version]="+default_wp_version;
    var total_pages;
    var api_obj;
    var plugins = [];

    onMount( async () => {
        const response = await fetch( freemius_plugins_lists );
        api_obj = await response.json();
        plugins = api_obj.plugins;
        next_page = api_obj.info + 1;
        total_pages = api_obj.info.pages;
        if ( current_page != 1 ){
            prev_page = next_page -1;
        }

        console.log( api_obj );
	});


    function openpage( page_slug ) {
        var url =  'https://wordpress.org/plugins/' + page_slug ;
        window.open(url, '_blank');
    }
</script>

  <div class="container">
    <div class="section">
      <div class="columns">
        <div class="column has-text-centered">
          <h1 class="title" ><strong>Freemdis </strong> <br/> Find easily WordPress products running with Freemius</h1><br>
        </div>
      </div>
      <div id="app" class="row columns is-multiline">
        {#each plugins as pg}
        
            <div class="column is-4" >
                <div key="{pg.id}"  class="card large">
                    <!-- <div class="card-image">
                    <figure class="image ">
                        <img src="" alt="Image-bo">
                    </figure>
                    </div> -->
                    <div class="card-content">
                    <div class="media">
                        <div class="media-left">
                        <figure class="image is-48x48">
                            <img src="{pg.icons['1x']}" alt="logo-{pg.slug}">
                        </figure>
                        </div>
                        <div class="media-content">
                        <p class="title is-4 no-padding">{@html pg.name}</p>
                        <p>
                            <span class="title is-6"></span> </p>
                            <p class="subtitle is-6">{@html pg.author}</p>
                        </div>
                    </div>
                    <div class="content">
                        {pg.short_description}
                        <div class="background-icon"><span class="icon-twitter"></span></div>
                    </div>
                    </div>
                </div>
                </div>
        {/each}
        <br/>
        <br/>

        <nav class="pagination is-centered" role="navigation" aria-label="pagination">
            {#if prev_page }
                <a class="pagination-previous">Previous</a>
            {/if}
            {#if next_page }
                <a class="pagination-next">Next page</a>
            {/if}
            <ul class="pagination-list">
                {#each Array(total_pages) as _, i}
                    <li><a class="pagination-link" data-page-id="{i+1}" aria-label="Goto page {i+1}">{i+1}</a></li>
               {/each}

        
            </ul>
          </nav>
      </div>
    </div>
  </div>

