<script lang="ts">
  import { onMount } from "svelte";
  import {PUBLIC_API_URL} from '$env/static/public';

  let centers: any[] = [];
  let loading: boolean = true;
  let error: string | null = null;
  let zipCode: string = "";

  onMount(async () => {
    const fetchCenters = async () => {
      try {
        loading = true; 
        const response = await fetch(`${PUBLIC_API_URL}/centers`);
        const result = await response.json();
        console.log(result);

        if (response.ok && result && result.data) { 
          centers = result.data; 
        } else {
          if (result && result.error && result.error.message) {
            error = result.error.message;
          } else {
            error = "Failed to load donation centers";
          }
        }
      } catch (err) {
        error = "Error connecting to the server";
        console.error("Error fetching centers:", err);
      } finally {
        loading = false; 
      }
    };

    fetchCenters();
  });

  // Function to determine background color based on center index

  const getBgColor = (index: number): string => {
    const colors = ["green", "blue", "purple", "yellow", "pink", "indigo"];
    return colors[index % colors.length];
  };

  // Function to create categories based on center accepts fields
  const getCategories = (center: any): string[] => {
    const categories: string[] = [];
    if (center.accepts_clothing) categories.push("Clothing");
    if (center.accepts_accessories) categories.push("Accessories");
    if (center.accepts_household) categories.push("Household");

    return categories.length ? categories : ["All Items"];
  };

  const handleTryAgain = () => {
    window.location.reload(); 
  };
</script>

<section id="donation-centers" class="py-16 md:py-24 bg-white dark:bg-neutral-800">
  <div class="container mx-auto px-4">
    <div class="text-center mb-16">
      <h2 class="text-3xl md:text-4xl font-bold text-gray-900 dark:text-white mb-4">
        Donation Centers
      </h2>
      <p class="text-lg text-gray-600 dark:text-gray-300 max-w-3xl mx-auto">
        Find a convenient drop-off location near you or schedule a pickup
        service for your donations.
      </p>
    </div>

    <div class="max-w-4xl mx-auto mb-16 bg-gray-50 dark:bg-neutral-700 rounded-xl shadow-md overflow-hidden">
      <div class="p-6 md:p-8">
        <h3 class="text-xl font-semibold text-gray-900 dark:text-white mb-4">
          Find a Donation Center
        </h3>
        <div class="flex flex-col md:flex-row space-y-4 md:space-y-0 md:space-x-4">
          <div class="flex-grow">
            <label
              for="location-search"
              class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
            >
              Enter your zip code or city
            </label>
            <input
              type="text"
              id="location-search"
              bind:value={zipCode}
              placeholder="e.g., 700135 or New Town"
              class="w-full px-4 py-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-neutral-800 text-gray-900 dark:text-white focus:outline-none focus:ring-2 focus:ring-green-500 dark:focus:ring-green-400"
            />
          </div>
          <div class="md:self-end">
            <button class="w-full md:w-auto px-6 py-2 bg-green-600 hover:bg-green-700 text-white font-medium rounded-lg transition duration-300">
              Find Centers
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Error State -->
    {#if error}
      <div class="text-center mb-8 p-4 bg-red-100 dark:bg-red-900 text-red-800 dark:text-red-200 rounded-lg">
        <p>{error}</p>
        <button
          on:click={handleTryAgain}
          class="mt-2 px-4 py-1 bg-red-200 dark:bg-red-800 rounded-md hover:bg-red-300 dark:hover:bg-red-700"
        >
          Try Again
        </button>
      </div>
    {/if}

    <!-- Loading State -->
    {#if loading}
      <div class="text-center mb-8">
        <div class="inline-block w-8 h-8 border-4 border-green-500 border-t-transparent rounded-full animate-spin"></div>
        <p class="mt-2 text-gray-600 dark:text-gray-300">
          Loading donation centers...
        </p>
      </div>
    {/if}

    <!-- Featured Locations -->
    {#if !loading && !error}
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        {#if centers.length > 0}
          {#each centers as center, index (center.id)}
            <div
              class="bg-white dark:bg-neutral-700 rounded-xl shadow-md overflow-hidden transition-transform duration-300 hover:shadow-lg hover:-translate-y-1"
            >
              <!-- Dynamic background color for header -->
              <div class={`bg-${getBgColor(index)}-600 px-4 py-2`}>
                <h3 class="text-white font-semibold">{center.name}</h3>
              </div>
              <div class="p-6">
                <div class="flex items-start mb-4">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class={`h-6 w-6 text-${getBgColor(index)}-600 dark:text-${getBgColor(index)}-400 mr-3 flex-shrink-0`}
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"
                    />
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"
                    />
                  </svg>
                  <address class="not-italic text-gray-700 dark:text-gray-200">
                    <strong>{center.name}</strong>
                    <br />
                    {center.address}
                    <br />
                    {center.city}, {center.state} {center.zip}
                  </address>
                </div>
                <div class="flex items-start mb-4">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class={`h-6 w-6 text-${getBgColor(index)}-600 dark:text-${getBgColor(index)}-400 mr-3 flex-shrink-0`}
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"
                    />
                  </svg>
                  <div class="text-gray-700 dark:text-gray-200">
                    <strong>Hours:</strong>
                    <br />
                    {center.hours}
                  </div>
                </div>
                <div class="flex items-start mb-6">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class={`h-6 w-6 text-${getBgColor(index)}-600 dark:text-${getBgColor(index)}-400 mr-3 flex-shrink-0`}
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z"
                    />
                  </svg>
                  <div class="text-gray-700 dark:text-gray-200">
                    <strong>Contact:</strong>
                    <br />
                    {center.phone}
                    {#if center.email}
                      <br />
                      {center.email}
                    {/if}
                  </div>
                </div>
                <div class="flex flex-wrap gap-2">
                  {#each getCategories(center) as category, idx (idx)}
                    <span
                      class={`px-3 py-1 bg-${category === "Clothing" ? "green" : category === "Accessories" ? "purple" : "blue"}-100 dark:bg-${category === "Clothing" ? "green" : category === "Accessories" ? "purple" : "blue"}-800 text-${category === "Clothing" ? "green" : category === "Accessories" ? "purple" : "blue"}-800 dark:text-${category === "Clothing" ? "green" : category === "Accessories" ? "purple" : "blue"}-100 text-sm rounded-full`}
                    >
                      {category}
                    </span>
                  {/each}
                </div>
              </div>
            </div>
          {/each}
        {:else}
          <div class="col-span-full text-center p-8 bg-gray-50 dark:bg-neutral-700 rounded-xl">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-12 w-12 mx-auto text-gray-400"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M20 13V6a2 2 0 00-2-2H6a2 2 0 00-2 2v7m16 0v5a2 2 0 01-2 2H6a2 2 0 01-2-2v-5m16 0h-2.586a1 1 0 00-.707.293l-2.414 2.414a1 1 0 01-.707.293h-3.172a1 1 0 01-.707-.293l-2.414-2.414A1 1 0 006.586 13H4"
              />
            </svg>
            <h3 class="mt-4 text-xl font-medium text-gray-900 dark:text-white">
              No donation centers found
            </h3>
            <p class="mt-2 text-gray-600 dark:text-gray-300">
              Please try a different search or check back later.
            </p>
          </div>
        {/if}
      </div>
    {/if}

    <!-- Partner Locations Section -->
    <div class="mt-16">
      <h3 class="text-2xl font-semibold text-gray-900 dark:text-white text-center mb-8">
        Our Partner Drop-off Locations
      </h3>
      <div class="bg-white dark:bg-neutral-700 rounded-xl shadow-md overflow-hidden">
        <div class="p-6 md:p-8">
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <div class="flex items-center p-4 bg-gray-50 dark:bg-neutral-600 rounded-lg">
              <div class="w-12 h-12 bg-blue-100 dark:bg-blue-900 rounded-full flex items-center justify-center mr-4">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-6 w-6 text-blue-600 dark:text-blue-400"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z"
                  />
                </svg>
              </div>
              <div>
                <h4 class="font-medium text-gray-900 dark:text-white">
                  Shopping Centers
                </h4>
                <p class="text-sm text-gray-600 dark:text-gray-300">
                  Look for our donation bins at major malls
                </p>
              </div>
            </div>

            <div class="flex items-center p-4 bg-gray-50 dark:bg-neutral-600 rounded-lg">
              <div class="w-12 h-12 bg-green-100 dark:bg-green-900 rounded-full flex items-center justify-center mr-4">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-6 w-6 text-green-600 dark:text-green-400"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"
                  />
                </svg>
              </div>
              <div>
                <h4 class="font-medium text-gray-900 dark:text-white">
                  Libraries
                </h4>
                <p class="text-sm text-gray-600 dark:text-gray-300">
                  Drop books and small items at local libraries
                </p>
              </div>
            </div>

            <div class="flex items-center p-4 bg-gray-50 dark:bg-neutral-600 rounded-lg">
              <div class="w-12 h-12 bg-yellow-100 dark:bg-yellow-900 rounded-full flex items-center justify-center mr-4">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-6 w-6 text-yellow-600 dark:text-yellow-400"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"
                  />
                </svg>
              </div>
              <div>
                <h4 class="font-medium text-gray-900 dark:text-white">
                  Schools
                </h4>
                <p class="text-sm text-gray-600 dark:text-gray-300">
                  Participating schools accept educational items
                </p>
              </div>
            </div>

            <div class="flex items-center p-4 bg-gray-50 dark:bg-neutral-600 rounded-lg">
              <div class="w-12 h-12 bg-purple-100 dark:bg-purple-900 rounded-full flex items-center justify-center mr-4">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-6 w-6 text-purple-600 dark:text-purple-400"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c1.657 0 3-4.03 3-9s-1.343-9-3-9m0 18c-1.657 0-3-4.03-3-9s1.343-9 3-9m-9 9a9 9 0 019-9"
                  />
                </svg>
              </div>
              <div>
                <h4 class="font-medium text-gray-900 dark:text-white">
                  Community Centers
                </h4>
                <p class="text-sm text-gray-600 dark:text-gray-300">
                  Various items accepted at local centers
                </p>
              </div>
            </div>

            <div class="flex items-center p-4 bg-gray-50 dark:bg-neutral-600 rounded-lg">
              <div class="w-12 h-12 bg-red-100 dark:bg-red-900 rounded-full flex items-center justify-center mr-4">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-6 w-6 text-red-600 dark:text-red-400"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"
                  />
                </svg>
              </div>
              <div>
                <h4 class="font-medium text-gray-900 dark:text-white">
                  Religious Centers
                </h4>
                <p class="text-sm text-gray-600 dark:text-gray-300">
                  Churches, temples and mosques participating
                </p>
              </div>
            </div>

            <div class="flex items-center p-4 bg-gray-50 dark:bg-neutral-600 rounded-lg">
              <div class="w-12 h-12 bg-pink-100 dark:bg-pink-900 rounded-full flex items-center justify-center mr-4">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-6 w-6 text-pink-600 dark:text-pink-400"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M13 10V3L4 14h7v7l9-11h-7z"
                  />
                </svg>
              </div>
              <div>
                <h4 class="font-medium text-gray-900 dark:text-white">
                  Pop-up Events
                </h4>
                <p class="text-sm text-gray-600 dark:text-gray-300">
                  Seasonal donation drives in various locations
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Call to Action for Pickup Service -->
    <div class="mt-16 text-center">
      <h3 class="text-2xl font-bold text-gray-900 dark:text-white mb-4">
        Unable to Visit a Donation Center?
      </h3>
      <p class="text-gray-600 dark:text-gray-300 mb-8 max-w-2xl mx-auto">
        We offer a convenient pickup service for larger donations or for
        those who cannot travel to our centers.
      </p>
      <a
        href="#pickup-service"
        class="inline-flex items-center px-6 py-3 bg-green-600 hover:bg-green-700 text-white font-medium rounded-lg transition duration-300 transform hover:scale-105"
      >
        Schedule a Pickup
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5 ml-2"
          viewBox="0 0 20 20"
          fill="currentColor"
        >
          <path
            fill-rule="evenodd"
            d="M10.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L12.586 11H5a1 1 0 110-2h7.586l-2.293-2.293a1 1 0 010-1.414z"
            clip-rule="evenodd"
          />
        </svg>
      </a>
    </div>
  </div>
</section>
