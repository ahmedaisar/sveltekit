<script context="module">
 
 import { api } from '../api/maldives';

 export const get = async ({ locals }) => {
	// locals.userid comes from src/hooks.js
	const response = await api('get');

	if (response.status === 404) {
		// user hasn't created a todo list.
		// start with an empty array`
		return {
			body: {
				hotels: []
			}
		};
	}

	if (response.status === 200) {
		return {
			body: {
				hotels: await response.json()
			}
		};
	}

	return {
		status: response.status
	};

    
};
 
</script>

<main id="main" class="site-main">
    <div class="site-banner">
        <div class="container">
            <div class="site-banner__content golo-ajax-search">
                <h1 class="site-banner__title">Explore the world</h1>
                <form action="/search" class="site-banner__search">
                    <div class="site-banner__search__field">
                        <span class="site-banner__search__icon">
                            <i class="la la-search la-24" />
                        </span><!-- .site-banner__search__icon -->
                        <input
                            class="site-banner__search__input"
                            type="text"
                            name="keyword"
                            placeholder="Type a city or location"
                            autocomplete="off"
                        />
                        <div class="search-result" />
                        <div class="golo-loading-effect">
                            <span class="golo-dual-ring" />
                        </div>
                    </div>
                    <!-- .site-banner__search__input -->
                </form>
                <!-- .site-banner__search -->
                <!-- lets edit -->
                <p class="site-banner__meta">
                    <span>Popular:</span>
                    {#await $hotels}
                    <div class="golo-loading-effect">
                        <span class="golo-dual-ring" />
                    </div>
                    {:then hotel}                  
                    {#each hotel.slice(100, 107) as hotel}
                            <a href="/hotels{hotel.localUrl}" title="{hotel.name}">{hotel.name}</a>
                     
                    {:else}
                        <p>loading...</p>
                    {/each}
                     {:catch error}
                    <p style="color: red">{error.message}</p>
                    {/await}
                </p>
                <!-- .site-banner__meta -->
            </div>
            <!-- .site-banner__content -->
        </div>
    </div>

    <div class="cities">
        <div class="container">
            <h2 class="cities__title title">Popular hotels</h2>
            <div class="cities__content">
                {#await $hotels}
                    <div class="golo-loading-effect">
                        <span class="golo-dual-ring" />
                    </div>
                {:then hotel}
                    <div class="row">
                        {#each hotel.slice(80,88 ) as hotel}
                      
                            <div class="col-lg-3 col-sm-6">
                                <div class="cities__item hover__box">
                                    <a
                                            title={hotel.name}
                                            href="/maldives{hotel.localUrl}"
                                        >
                                    <div
                                        class="cities__thumb hover__box__thumb"
                                    >
                                        
                                            <img
                                                src={hotel.pictures[0].url}
                                                alt={hotel.name}
                                            />
                                        
                                    </div>
                                    <h4 class="cities__name">{hotel.name}</h4>
                                    <div class="cities__info">
                                        <h3 class="cities__capital" style="font-size: 18px">
                                            {hotel.name}
                                        </h3>
                                        <p class="cities__number">
                                            {hotel.standard} | {hotel.transfer
                                                ? hotel.transfer
                                                : ""}
                                        </p>
                                    </div>
                                </a>
                                </div>
                            </div>
                        {:else}
                            <p>loading...</p>
                        {/each}
                    </div>
                {:catch error}
                    <p style="color: red">{error.message}</p>
                {/await}
            </div>
            <!-- .cities__content -->
        </div>
    </div>
    <!-- on:success listens for dispatched 'success' events -->
    <!-- <Login on:success={redirectToProfile} />
    <Register on:success={redirectToProfile} /> -->
</main>
