<script lang="ts">
	import IconDrawerLeft from '$lib/assets/icons/drawer/IconDrawerLeft.svelte'
	import IconDrawerChevron from '$lib/assets/icons/drawer/IconDrawerChevron.svelte'
	import LoadingItemCarousel from '$lib/components/Browse/Sections/LoadingItemCarousel.svelte'
	import { onMount } from 'svelte'
	import Swiper, { Navigation } from 'swiper'
	import 'swiper/css'
	import SectionCarouselItem from '$lib/components/Browse/Sections/SectionCarouselItem.svelte'

	export let channels: any = []
	let swiper: Swiper

	onMount(() => {
		swiper = new Swiper('.carousel', {
			slidesPerView: 3,
			spaceBetween: 15,
			loop: true,
			modules: [Navigation],
			navigation: {
				nextEl: '.btn-next',
				prevEl: '.btn-prev'
			},
			breakpoints: {
				320: {
					slidesPerView: 1
				},
				480: {
					slidesPerView: 2
				},
				800: {
					slidesPerView: 3
				}
			}
		})
	})
</script>

{#await channels}
	<div class="flex flex-col my-4 relative overflow-x-auto scrollbar-hide">
		<div role="status" class="flex flex-row gap-1 animate-pulse">
			{#each Array(5) as _, index (index)}
				<LoadingItemCarousel />
			{/each}
		</div>
	</div>
{:then value}
	{#if value.length > 0}
		<div class="relative p-1">
			<div class="btn btn-square p-3 btn-prev absolute top-2/4 left-1 z-10 ml-3">
				<IconDrawerLeft />
			</div>
			<div class="swiper carousel mt-10 mx-8">
				<div class="swiper-wrapper">
					{#each value as channel}
						<SectionCarouselItem {channel} />
					{/each}
				</div>
			</div>
			<div class="btn btn-square z-10 p-3 btn-next absolute top-2/4 right-1 mr-3">
				<IconDrawerChevron />
			</div>
		</div>
	{/if}
{/await}

<style>
	.swiper-slide {
		background: #fff;
		display: flex;
	}
	:global(.swiper-slide) {
		height: 20rem !important;
	}

	.scrollbar-hide::-webkit-scrollbar {
		display: none;
	}

	/* For IE, Edge and Firefox */
	.scrollbar-hide {
		-ms-overflow-style: none;
		scrollbar-width: none;
	}
</style>
