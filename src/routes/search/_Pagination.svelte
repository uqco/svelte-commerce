<style>
.active {
	color: #fff;
	background-color: #282c3f;
}
</style>

<script>
import { goto } from '$app/navigation'
import { page } from '$app/stores'
import { createEventDispatcher } from 'svelte'

const dispatch = createEventDispatcher()

export let count = 10,
	current = 1
count = +count
$: pages = +count
$: startTab = 5 - current <= 5 && 5 - current >= 0 ? 1 : current - 4
$: endTab = startTab + 9

function changePage(e) {
	current = e || '1'
	// dispatch('change', current)
	// $page.url.searchParams.set('page', current.toString())
	// let query = {}
	let u = new URL($page.url)
	u.searchParams.set('page', current.toString())
	goto(u.toString())
	window.scroll({ top: 0, behavior: 'smooth' })
	// goto($page.url.pathname + $page.url.search)
	// const r = $page.url.searchParams.get('r')
	// console.log('rrrrrrrrrrrrrrr', r)
	// if (r) $page.url.searchParams.delete('r')
	// else $page.url.searchParams.set('r', Math.random().toString())
	// // scrollToTop();
}
</script>

{#if count > 1}
	<div class="lg:flex items-center justify-between border-t border-gray-200 lg:pt-5">
		<div class="items-center text-gray-500 py-4 text-xs sm:text-base whitespace-nowrap">
			Page {current} of {count}
		</div>

		<div class="xl:mx-2 flex justify-center space-x-2 items-center text-center">
			{#if +current > 1}
				<button
					class="inline-flex items-center p-0.5 sm:p-2 font-bold tracking-wide text-gray-800 bg-white border border-gray-300 rounded hover:border-gray-800 lg:py-2 lg:px-4 focus:outline-none"
					on:click="{() => changePage(+current - 1)}">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="h-5 w-5 sm:mr-2"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor">
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M15 19l-7-7 7-7"></path>
					</svg>

					<span class="hidden sm:block text-xs">Previous</span>
				</button>
			{/if}

			<div class="flex flex-wrap space-x-1">
				{#each { length: pages } as _, i}
					{#if startTab <= i && endTab - 1 >= i}
						<button
							class="h-5 w-5 sm:h-8 sm:w-8 text-xs sm:text-base rounded hover:border border-gray-800 mx-auto hover:border-gray-800 focus:outline-none"
							class:active="{+current === i}"
							on:click="{() => changePage(i)}">
							{i}
						</button>
					{/if}
				{/each}
			</div>

			{#if +current < count}
				<button
					class="inline-flex items-center p-0.5 sm:p-2 font-bold tracking-wide text-center text-gray-800 bg-white border border-gray-300 rounded  hover:border-gray-800 lg:py-2 lg:px-4 focus:outline-none"
					on:click="{() => changePage(+current + 1)}">
					<span class="hidden sm:block text-xs">Next</span>

					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="h-5 w-5 sm:ml-2"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"
						></path>
					</svg>
				</button>
			{/if}
		</div>
	</div>
{/if}
