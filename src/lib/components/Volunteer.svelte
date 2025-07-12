<script>
  import { slide } from 'svelte/transition';
  import {PUBLIC_API_URL} from '$env/static/public';
  let formData = {
    name: "",
    email: "",
    phone: "",
    address: "",
    city: "",
    state: "",
    zip: "",
    availability: "",
    skills: "",
    interests: "",
    previous_experience: "",
    preferred_center_id: 1,
  };

  let formStatus = {
    isSubmitting: false,
    isSuccess: false,
    isError: false,
    message: "",
  };

  let termsAccepted = false;

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    formData = {
      ...formData,
      [name]: value,
    };
  };

  const handleInterestsChange = (e) => {
    // Get all selected options
    const selectedOptions = Array.from(e.target.selectedOptions).map(
      (option) => option.value,
    );
    formData = {
      ...formData,
      interests: selectedOptions.join(", "),
    };
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    if (!termsAccepted) {
      formStatus = {
        isSubmitting: false,
        isSuccess: false,
        isError: true,
        message: "Please accept the volunteer agreement to continue.",
      };
      return;
    }

    // Combine first and last name from the form inputs directly
    const firstNameInput = document.getElementById("volunteer-first-name");
    const lastNameInput = document.getElementById("volunteer-last-name");
    const fullName = `${firstNameInput ? firstNameInput.value : ''} ${lastNameInput ? lastNameInput.value : ''}`;

    const payload = {
      ...formData,
      name: fullName,
      availability: document.getElementById("volunteer-availability").value,
      // Convert preferred_center_id to number if it existsp
      preferred_center_id: formData.preferred_center_id
        ? Number(formData.preferred_center_id)
        : null,
    };

    formStatus = {
      isSubmitting: true,
      isSuccess: false,
      isError: false,
      message: "",
    };

    try {
      const response = await fetch(`${PUBLIC_API_URL}/volunteers`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(formData),
              });


      if (response.ok) {
        formStatus = {
          isSubmitting: false,
          isSuccess: true,
          isError: false,
          message: "Thank you for your application! We will contact you soon.",
        };

        formData = {
          name: "",
          email: "",
          phone: "",
          address: "",
          city: "",
          state: "",
          zip: "",
          availability: "",
          skills: "",
          interests: "",
          previous_experience: "",
          preferred_center_id: null,
        };
        termsAccepted = false;

        // Manually reset input fields that are not directly bound to formData
        if (firstNameInput) firstNameInput.value = "";
        if (lastNameInput) lastNameInput.value = "";
        const availabilityInput = document.getElementById("volunteer-availability");
        if (availabilityInput) availabilityInput.value = "";

      } else {
        throw new Error(
          "An error occurred while submitting your application.",
        );
      }
    } catch (error) {
      formStatus = {
        isSubmitting: false,
        isSuccess: false,
        isError: true,
        message:
          error.message ||
          "Failed to submit your application. Please try again later.",
      };
    }
  };
</script>

<section
  id="volunteer"
  class="py-16 md:py-24 bg-white dark:bg-neutral-800"
>
  <div class="container mx-auto px-4">
    <div class="max-w-4xl mx-auto">
      <div class="text-center mb-16">
        <h2 class="text-3xl md:text-4xl font-bold text-gray-900 dark:text-white mb-4">
          Volunteer With Us
        </h2>
        <p class="text-lg text-gray-600 dark:text-gray-300 max-w-3xl mx-auto">
          Join our team of dedicated volunteers and make a hands-on
          difference in your community.
        </p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
        <div class="order-2 md:order-1">
          <h3 class="text-2xl font-bold text-gray-900 dark:text-white mb-4">
            Why Volunteer?
          </h3>
          <p class="text-gray-700 dark:text-gray-300 mb-6">
            Volunteering is a rewarding experience that allows you to
            connect with your community, learn new skills, and contribute to
            a cause you care about.
          </p>
          <ul class="space-y-4">
            <li class="flex items-start">
              <div class="flex-shrink-0 h-6 w-6 bg-green-100 dark:bg-green-900/30 rounded-full flex items-center justify-center mr-3">
                <svg
                  class="h-4 w-4 text-green-600 dark:text-green-400"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                >
                  <path
                    fill-rule="evenodd"
                    d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                    clip-rule="evenodd"
                  />
                </svg>
              </div>
              <p class="text-gray-700 dark:text-gray-300">
                <strong class="font-semibold text-gray-900 dark:text-white">
                  Meaningful Impact:
                </strong>{" "}
                Directly help individuals and families in need.
              </p>
            </li>
            <li class="flex items-start">
              <div class="flex-shrink-0 h-6 w-6 bg-green-100 dark:bg-green-900/30 rounded-full flex items-center justify-center mr-3">
                <svg
                  class="h-4 w-4 text-green-600 dark:text-green-400"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                >
                  <path
                    fill-rule="evenodd"
                    d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                    clip-rule="evenodd"
                  />
                </svg>
              </div>
              <p class="text-gray-700 dark:text-gray-300">
                <strong class="font-semibold text-gray-900 dark:text-white">
                  Community:
                </strong>{" "}
                Meet like-minded people and build lasting friendships.
              </p>
            </li>
            <li class="flex items-start">
              <div class="flex-shrink-0 h-6 w-6 bg-green-100 dark:bg-green-900/30 rounded-full flex items-center justify-center mr-3">
                <svg
                  class="h-4 w-4 text-green-600 dark:text-green-400"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                >
                  <path
                    fill-rule="evenodd"
                    d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                    clip-rule="evenodd"
                  />
                </svg>
              </div>
              <p class="text-gray-700 dark:text-gray-300">
                <strong class="font-semibold text-gray-900 dark:text-white">
                  Skill Development:
                </strong>{" "}
                Gain experience in logistics, customer service, and more.
              </p>
            </li>
          </ul>
        </div>
        <div class="order-1 md:order-2">
          <div class="relative">
            <div class="aspect-w-4 aspect-h-3">
              <div class="bg-gray-200 dark:bg-neutral-700 rounded-xl shadow-lg flex items-center justify-center">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-24 w-24 text-green-500 dark:text-green-400"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"
                  />
                </svg>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="mt-16 bg-gray-50 dark:bg-neutral-700 rounded-xl shadow-md overflow-hidden">
        <div class="p-8">
          <h3 class="text-2xl font-bold text-gray-900 dark:text-white mb-6 text-center">
            Volunteer Application
          </h3>

          {#if formStatus.isSuccess}
            <div class="p-4 bg-green-100 dark:bg-green-900/30 rounded-lg text-center" transition:slide>
              <p class="text-green-700 dark:text-green-300 font-medium">
                {formStatus.message}
              </p>
              <button
                on:click={() => formStatus = { ...formStatus, isSuccess: false }}
                class="mt-4 py-2 px-4 bg-green-600 hover:bg-green-700 text-white font-medium rounded-lg transition duration-300"
              >
                Submit Another Application
              </button>
            </div>
          {:else}
            <form class="space-y-6" on:submit={handleSubmit}>
              {#if formStatus.isError}
                <div class="p-4 bg-red-100 dark:bg-red-900/30 rounded-lg" transition:slide>
                  <p class="text-red-700 dark:text-red-300">
                    {formStatus.message}
                  </p>
                </div>
              {/if}

              <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div>
                  <label
                    for="volunteer-first-name"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    First Name
                  </label>
                  <input
                    type="text"
                    id="volunteer-first-name"
                    name="volunteer-first-name"
                    value=""
                    required
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  />
                </div>
                <div>
                  <label
                    for="volunteer-last-name"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    Last Name
                  </label>
                  <input
                    type="text"
                    id="volunteer-last-name"
                    name="volunteer-last-name"
                    value=""
                    required
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  />
                </div>
              </div>

              <div>
                <label
                  for="email"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Email Address
                </label>
                <input
                  type="email"
                  id="email"
                  name="email"
                  value=""
                  required
                  on:input={handleInputChange}
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                />
              </div>

              <div>
                <label
                  for="phone"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Phone Number
                </label>
                <input
                  type="tel"
                  id="phone"
                  name="phone"
                  value=""
                  on:input={handleInputChange}
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                />
              </div>

              <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div>
                  <label
                    for="address"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    Address
                  </label>
                  <input
                    type="text"
                    id="address"
                    name="address"
                    on:input={handleInputChange}
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  />
                </div>
                <div>
                  <label
                    for="city"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    City
                  </label>
                  <input
                    type="text"
                    id="city"
                    name="city"
                    on:input={handleInputChange}
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  />
                </div>
              </div>

              <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div>
                  <label
                    for="state"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    State
                  </label>
                  <input
                    type="text"
                    id="state"
                    name="state"
                    on:input={handleInputChange}
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  />
                </div>
                <div>
                  <label
                    for="zip"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    ZIP Code
                  </label>
                  <input
                    type="text"
                    id="zip"
                    name="zip"
                    on:input={handleInputChange}
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  />
                </div>
              </div>

              <div>
                <label
                  for="skills"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Skills (Optional)
                </label>
                <input
                  type="text"
                  id="skills"
                  name="skills"
                  placeholder="e.g., Organizing, Customer Service, Driving"
                  on:input={handleInputChange}
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                />
              </div>

              <div>
                <label
                  for="volunteer-interests"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Areas of Interest
                </label>
                <select
                  id="volunteer-interests"
                  name="volunteer-interests"
                  multiple
                  required
                  on:change={handleInterestsChange}
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  size="4"
                >
                  <option value="Sorting Donations">
                    Sorting Donations
                  </option>
                  <option value="Donation Pickups">Donation Pickups</option>
                  <option value="Community Events">Community Events</option>
                  <option value="Administrative Support">
                    Administrative Support
                  </option>
                </select>
                <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">
                  Hold Ctrl/Cmd to select multiple interests
                </p>
              </div>

              <div>
                <label
                  for="volunteer-availability"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Availability
                </label>
                <textarea
                  id="volunteer-availability"
                  name="volunteer-availability"
                  rows="3"
                  required
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  placeholder="e.g., Weekday mornings, weekends, etc."
                ></textarea>
              </div>

              <div>
                <label
                  for="previous_experience"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Previous Volunteer Experience (Optional)
                </label>
                <textarea
                  id="previous_experience"
                  name="previous_experience"
                  rows="3"
                  on:input={handleInputChange}
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  placeholder="Tell us about any previous volunteer experience you have"
                ></textarea>
              </div>

              <div>
                <label
                  for="preferred_center_id"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Preferred Donation Center (Optional)
                </label>
                <select
                  id="preferred_center_id"
                  name="preferred_center_id"
                  on:input={handleInputChange}
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                >
                  <option value="">No preference</option>
                  <option value="1">Community Aid Center</option>
                  <option value="2">Downtown Donation Hub</option>
                  <option value="3">Eastside Collection Center</option>
                </select>
              </div>

              <div class="flex items-start">
                <div class="flex items-center h-5">
                  <input
                    id="volunteer-terms"
                    name="volunteer-terms"
                    type="checkbox"
                    bind:checked={termsAccepted}
                    class="h-4 w-4 text-green-600 focus:ring-green-500 border-gray-300 rounded"
                  />
                </div>
                <div class="ml-3 text-sm">
                  <label
                    for="volunteer-terms"
                    class="font-medium text-gray-700 dark:text-gray-300"
                  >
                    I am over 18 and agree to the{" "}
                    <a
                      href="#"
                      class="text-green-600 hover:text-green-500"
                    >
                      volunteer agreement
                    </a>
                  </label>
                </div>
              </div>

              <div>
                <button
                  type="submit"
                  disabled={formStatus.isSubmitting}
                  class={`w-full py-3 px-4 bg-green-600 hover:bg-green-700 text-white font-medium rounded-lg transition duration-300 transform hover:scale-105 ${
                    formStatus.isSubmitting
                      ? "opacity-70 cursor-not-allowed"
                      : ""
                  }`}
                >
                  {#if formStatus.isSubmitting}
                    Submitting...
                  {:else}
                    Submit Application
                  {/if}
                </button>
              </div>
            </form>
          {/if}
        </div>
      </div>
    </div>
  </div>
</section>
