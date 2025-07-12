<script>
	let isMenuOpen = false;

	function toggleMenu() {
		isMenuOpen = !isMenuOpen;
	}
</script>

<header id="header" class="fixed top-0 z-20 w-full bg-white shadow-md dark:bg-neutral-900">
	<nav class="container mx-auto flex items-center justify-between px-4 py-3">
		<div class="flex items-center">
			<a href="#hero" class="text-primary-600 flex items-center text-2xl font-bold dark:text-white">
				<span class="text-4CAF50">Aid</span><span class="text-2196F3">Link</span>
			</a>
		</div>

		<!-- Desktop Navigation -->
		<ul class="hidden items-center space-x-6 md:flex">
			<li>
				<a
					href="#hero"
					class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
				>
					Home
				</a>
			</li>
			<li>
				<a
					href="#how-it-works"
					class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
				>
					How It Works
				</a>
			</li>
			<li>
				<a
					href="#donation-items"
					class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
				>
					Donation Items
				</a>
			</li>
			<li>
				<a
					href="#impact-stories"
					class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
				>
					Impact Stories
				</a>
			</li>
			<li>
				<a
					href="#donation-centers"
					class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
				>
					Donation Centers
				</a>
			</li>
			<li>
				<a
					href="#pickup-service"
					class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
				>
					Pickup Service
				</a>
			</li>
			<li>
				<a
					href="#faq"
					class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
				>
					FAQ
				</a>
			</li>
			<li>
				<a
					href="#volunteer"
					class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
				>
					Volunteer
				</a>
			</li>
			<li>
				<a
					href="#contact"
					class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
				>
					Contact
				</a>
			</li>
			<li>
				<a
					href="#contact"
					class="hover:bg-opacity-90 rounded-lg bg-green-500 px-4 py-2 text-white transition duration-300"
				>
					Donate Now
				</a>
			</li>
		</ul>

		<!-- Mobile Navigation Toggle -->
		<button
			aria-label="Toggle Menu"
			class="text-gray-700 focus:outline-none md:hidden dark:text-gray-200"
			on:click={toggleMenu}
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
					d="M4 6h16M4 12h16M4 18h16"
				/>
			</svg>
		</button>

		<!-- Mobile Navigation Menu -->
		<div
			class:hidden={!isMenuOpen}
			class="fixed inset-0 z-50 flex flex-col items-center justify-center bg-white dark:bg-neutral-900"
		>
			<div class="absolute top-0 right-0 p-4">
				<button
					aria-label="Close Menu"
					class="text-gray-700 focus:outline-none dark:text-gray-200"
					on:click={toggleMenu}
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
							d="M6 18L18 6M6 6l12 12"
						/>
					</svg>
				</button>
			</div>
			<ul class="flex flex-col items-center space-y-6 text-xl">
				<li>
					<a
						href="#hero"
						on:click={toggleMenu}
						class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
					>
						Home
					</a>
				</li>
				<li>
					<a
						href="#how-it-works"
						on:click={toggleMenu}
						class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
					>
						How It Works
					</a>
				</li>
				<li>
					<a
						href="#donation-items"
						on:click={toggleMenu}
						class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
					>
						Donation Items
					</a>
				</li>
				<li>
					<a
						href="#impact-stories"
						on:click={toggleMenu}
						class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
					>
						Impact Stories
					</a>
				</li>
				<li>
					<a
						href="#donation-centers"
						on:click={toggleMenu}
						class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
					>
						Donation Centers
					</a>
				</li>
				<li>
					<a
						href="#pickup-service"
						on:click={toggleMenu}
						class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
					>
						Pickup Service
					</a>
				</li>
				<li>
					<a
						href="#faq"
						on:click={toggleMenu}
						class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
					>
						FAQ
					</a>
				</li>
				<li>
					<a
						href="#volunteer"
						on:click={toggleMenu}
						class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
					>
						Volunteer
					</a>
				</li>
				<li>
					<a
						href="#contact"
						on:click={toggleMenu}
						class="hover:text-4CAF50 dark:hover:text-4CAF50 text-gray-700 transition duration-300 dark:text-gray-200"
					>
						Contact
					</a>
				</li>
				<li>
					<a
						href="#contact"
						on:click={toggleMenu}
						class="hover:bg-opacity-90 w-full rounded-lg bg-green-500 px-6 py-3 text-center text-white transition duration-300"
					>
						Donate Now
					</a>
				</li>
			</ul>
		</div>
	</nav>
</header>
