<script>
	let activeSlide = 0;
	const testimonials = [
		{
			name: 'Maria Rodriguez',
			title: 'Single Mother of Three',
			image: 'M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z',
			quote:
				'After losing my job during the pandemic, I struggled to provide essentials for my children. The clothing and school supplies we received from your organization made an incredible difference. My kids were able to start the school year with confidence, and I found a new job shortly after. The support came at exactly the right time.',
			tags: ['School Supplies', 'Winter Clothing', 'Household Items'],
			color: 'green'
		},
		{
			name: 'James Wilson',
			title: 'Veteran Housing Program',
			image: 'M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z',
			quote:
				'Our veteran housing program serves dozens of former service members transitioning to civilian life. The consistent donations of professional clothing, household essentials, and personal care items have been vital for our residents. Many have secured employment and their own housing thanks to having appropriate attire for interviews and the dignity that comes with these basic necessities.',
			tags: ['Professional Clothing', 'Household Goods', 'Personal Care'],
			color: 'blue'
		},
		{
			name: 'New Beginnings',
			title: 'Community Shelter',
			image:
				'M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4',
			quote:
				"Our shelter assists families who have lost their homes due to natural disasters, financial hardship, or other crises. The donations we receive provide immediate comfort during traumatic times. Children's toys and books have been especially meaningful, giving kids a sense of normalcy and joy during upheaval. The bedding, clothing, and kitchen essentials help families rebuild their lives from scratch.",
			tags: ["Children's Items", 'Bedding & Linens', 'Kitchen Essentials'],
			color: 'purple'
		}
	];
	const slides = testimonials.length;

	function next() {
		activeSlide = (activeSlide + 1) % slides;
	}

	function prev() {
		activeSlide = (activeSlide - 1 + slides) % slides;
	}
</script>

<section id="impact-stories" class="bg-gray-50 py-16 md:py-24 dark:bg-neutral-900">
	<div class="container mx-auto px-4">
		<div class="mb-16 text-center">
			<h2 class="mb-4 text-3xl font-bold text-gray-900 md:text-4xl dark:text-white">
				Impact Stories
			</h2>
			<p class="mx-auto max-w-3xl text-lg text-gray-600 dark:text-gray-300">
				See how your donations are making a real difference in the lives of people in our community.
			</p>
		</div>

		<div class="relative mx-auto max-w-5xl">
			<div class="relative overflow-hidden">
				<div
					class="flex transition-transform duration-500 ease-in-out"
					style="transform: translateX(-{activeSlide * 100}%);"
				>
					{#each testimonials as testimonial (testimonial.name)}
						<div class="w-full flex-shrink-0 p-4">
							<div class="overflow-hidden rounded-xl bg-white shadow-lg dark:bg-neutral-800">
								<div class="md:flex">
									<div
										class="md:w-1/3 bg-{testimonial.color}-600 flex flex-col items-center justify-center p-8 text-center text-white"
									>
										<div
											class="mb-4 flex h-24 w-24 items-center justify-center rounded-full bg-white/20"
										>
											<svg
												xmlns="http://www.w3.org/2000/svg"
												class="h-12 w-12"
												fill="none"
												viewBox="0 0 24 24"
												stroke="currentColor"
											>
												<path
													stroke-linecap="round"
													stroke-linejoin="round"
													stroke-width="2"
													d={testimonial.image}
												/>
											</svg>
										</div>
										<h3 class="mb-1 text-xl font-semibold">{testimonial.name}</h3>
										<p class="text-sm text-white/80">{testimonial.title}</p>
									</div>
									<div class="p-8 md:w-2/3">
										<div class="mb-4 flex items-start">
											<svg
												xmlns="http://www.w3.org/2000/svg"
												class="h-10 w-10 text-{testimonial.color}-600 dark:text-{testimonial.color}-400 mr-4"
												fill="currentColor"
												viewBox="0 0 24 24"
											>
												<path
													d="M14.017 21v-7.391c0-5.704 3.731-9.57 8.983-10.609l.995 2.151c-2.432.917-3.995 3.638-3.995 5.849h4v10h-9.983zm-14.017 0v-7.391c0-5.704 3.748-9.57 9-10.609l.996 2.151c-2.433.917-3.996 3.638-3.996 5.849h3.983v10h-9.983z"
												/>
											</svg>
											<p class="leading-relaxed text-gray-700 dark:text-gray-300">
												{testimonial.quote}
											</p>
										</div>
										<div class="mt-6">
											<h4 class="font-semibold text-gray-900 dark:text-white">Impact:</h4>
											<div class="mt-2 flex flex-wrap gap-2">
												{#each testimonial.tags as tag (tag)}
													<span
														class="rounded-full bg-green-100 px-3 py-1 text-sm text-green-800 dark:bg-green-800 dark:text-green-100"
														>{tag}</span
													>
												{/each}
											</div>
										</div>
									</div>
								</div>
							</div>
						</div>
					{/each}
				</div>
			</div>

			<button
				on:click={prev}
				aria-label="Previous testimonial"
				class="absolute top-1/2 left-0 z-10 -translate-x-2 -translate-y-1/2 transform rounded-full bg-white p-2 text-gray-800 shadow-lg transition-all hover:bg-gray-100 focus:ring-2 focus:ring-green-500 focus:outline-none lg:translate-x-0 dark:bg-neutral-700 dark:text-gray-200 dark:hover:bg-neutral-600"
			>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					class="h-6 w-6"
					fill="none"
					viewBox="0 0 24 24"
					stroke="currentColor"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M15 19l-7-7 7-7"
					/>
				</svg>
			</button>
			<button
				on:click={next}
				aria-label="Next testimonial"
				class="absolute top-1/2 right-0 z-10 translate-x-2 -translate-y-1/2 transform rounded-full bg-white p-2 text-gray-800 shadow-lg transition-all hover:bg-gray-100 focus:ring-2 focus:ring-green-500 focus:outline-none lg:translate-x-0 dark:bg-neutral-700 dark:text-gray-200 dark:hover:bg-neutral-600"
			>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					class="h-6 w-6"
					fill="none"
					viewBox="0 0 24 24"
					stroke="currentColor"
				>
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
				</svg>
			</button>
		</div>
	</div>
</section>
