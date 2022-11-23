<script>
    import { metatags } from '@roxi/routify';
    import { onMount } from 'svelte';
    import generateAvatar from "github-like-avatar-generator";

    
    metatags.title = 'Freemdis : Freemius WordPress Directory';
    metatags.description = 'Find easily WordPress products running with Freemius';

    var current_page = 1;
    var page_limit   = 30;
    var next_page;
    var prev_page;
    var default_wp_version = '6.1';
    var freemius_plugins_lists = "https://api.wordpress.org/plugins/info/1.2/?action=query_plugins&request[page]="+current_page+"&request[per_page]="+page_limit+"&request[author]=freemius;
    var total_pages;
    var api_obj;
    var plugins = [];


    function genAvatar(){
        var avatar = generateAvatar({
            blocks: 6, // must be multiple of two
            width: 100
        });
        const image = document.createElement("img");
        image.src = avatar.base64;
        return image.outerHTML;
    }


    function generateURL( page_number, page_limit, default_wp_version ) {
        return freemius_plugins_lists = "https://api.wordpress.org/plugins/info/1.2/?action=query_plugins&request[page]="+page_number+"&request[per_page]="+page_limit+"&request[author]=freemius&request[wp_version]="+default_wp_version;
    }

    onMount( async () => {
        const response = await fetch( generateURL( current_page, page_limit, default_wp_version ) );
        api_obj = await response.json();
        plugins = api_obj.plugins;
        next_page = api_obj.info + 1;
        total_pages = api_obj.info.pages;
        if ( current_page != 1 ){
            prev_page = next_page -1;
        }
	});


    function openpage( page_slug ) {
        var url =  'https://wordpress.org/plugins/' + page_slug ;
        window.open(url, '_blank');
    }


    function navigatePage( page_number ) {
        const response = fetch( generateURL( page_number, page_limit, default_wp_version ) );
        api_obj = response.json();
        plugins = api_obj.plugins;
    }

    async function gotopage( page_number ) {
        const response = await fetch( generateURL( page_number, page_limit, default_wp_version ) );
        api_obj = await response.json();
        plugins = api_obj.plugins;
       
       
    }



</script>

  <div class="container">
    <div class="section">
      <div class="columns">
        <div class="column has-text-centered">
          <h1 class="title" ><strong>Freemdis </strong> <br/> Find easily WordPress products running with Freemius</h1><br>
        </div>
      </div>
      <br/>
      <nav class="pagination is-centered" role="navigation" aria-label="pagination">
        <ul class="pagination-list">
            {#each Array(total_pages) as _, i}
                <li><a class="pagination-link" href="/" data-page-id="{i+1}" on:click={ () =>  gotopage(i+1)} aria-label="Goto page {i+1}">{i+1}</a></li>
            {/each}
        </ul>
    </nav>
      <div id="app" class="row columns is-multiline">
        {#each plugins as pg}
        
            <div class="column is-4" >
                <div key="{pg.id}"  class="card large">
                    <!-- <div class="card-image">
                    <figure class="image ">
                        <img src="" alt="Image-bo">
                    </figure>
                    </div> -->
                    <div class="card-content" on:click={ () => openpage( pg.slug ) } style="cursor:pointer;">
                    <div class="media">
                        <div class="media-left">
                        <figure class="image is-48x48">
                            {#if pg.icons['1x'] }
                                <img src="{ pg.icons['1x'] }" alt="logo-{pg.slug}">
                            {:else}
                                {@html genAvatar()}
                            {/if}
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
      </div>
      <br/>
      <br/>
      <br/>
      <br/>
      <nav class="pagination is-centered" role="navigation" aria-label="pagination">
        <ul class="pagination-list">
            {#each Array(total_pages) as _, i}
                <li><a class="pagination-link" href="/" data-page-id="{i+1}" on:click={ () =>  gotopage(i+1)} aria-label="Goto page {i+1}">{i+1}</a></li>
           {/each}
        </ul>
    </nav>
    </div>
  </div>

