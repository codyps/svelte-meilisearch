<script>
	import instantsearch from 'instantsearch.js'
	import { searchBox, hits } from 'instantsearch.js/es/widgets'
	import { instantMeiliSearch } from '@meilisearch/instant-meilisearch'
	import { onMount } from 'svelte'
	
	onMount(() => {
		const searchAdapter = instantMeiliSearch(
      'https://ms-adf78ae33284-106.lon.meilisearch.io',
      'a63da4928426f12639e19d62886f621130f3fa9ff3c7534c5d179f0f51c4f303'
    );
		
		const searchClient = searchAdapter.searchClient

		const search = instantsearch({
			searchClient,
			indexName: 'steam-videogames',
      future: {
        preserveSharedStateOnUnmount: true,
      },
		})
		search.addWidgets([
			searchBox({
				container: '#searchbox',
			}),
			hits({
				container: '#hits',
				templates: {
					item(hit, {html, components}) {
            return html`
              <article key=${hit.id}>
                <img src=${hit.image} alt=${hit.name} />
                <h1>${hit.name}</h1>
                <p>${hit.description}</p>
              </article>
            `
          }
				}
			})
		])

		search.start()
})
</script>

<div id="searchbox"></div>
<div id="hits"></div>
