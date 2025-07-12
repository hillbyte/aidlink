<script>
  import { onMount } from 'svelte';
  import { slide } from 'svelte/transition';
   import {
PUBLIC_API_URL
	} from '$env/static/public';

  let openFaq = null;
  let isSubmitting = false;
  let formError = "";
  let formSuccess = false;

  let formData = {
    donor_name: "",
    donor_email: "",
    donor_phone: "",
    donation_type: "",
    items_description: "",
    quantity: null,
    condition: "good",
    pickup_required: true,
    pickup_address: "",
    pickup_city: "",
    pickup_state: "",
    pickup_zip: "",
    preferred_date: "",
    preferred_time: "",
    notes: "",
    terms: false,
  };

  const handleChange = (event) => {
    const { name, value, type, checked } = event.target;

    if (type === "checkbox") {
      formData = { ...formData, [name]: checked };
    } else {
      formData = { ...formData, [name]: value };
    }
  };

  const handleMultiSelect = (event) => {
    const options = event.target.options;
    const selectedValues = [];

    for (let i = 0; i < options.length; i++) {
      if (options[i].selected) {
        selectedValues.push(options[i].value);
      }
    }

    formData = { ...formData, donation_type: selectedValues.join(", ") };
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    if (
      !formData.donor_name ||
      !formData.donor_email ||
      !formData.pickup_address ||
      !formData.pickup_city ||
      !formData.pickup_state ||
      !formData.pickup_zip ||
      !formData.preferred_date ||
      !formData.preferred_time
    ) {
      formError = "Please fill in all required fields";
      return;
    }

    if (!formData.terms) {
      formError = "Please agree to the terms and conditions";
      return;
    }

    isSubmitting = true;
    formError = "";
    formSuccess = false;

    try {
      const donationData = {
        donor_name: formData.donor_name,
        donor_email: formData.donor_email,
        donor_phone: formData.donor_phone || null,
        donation_type: formData.donation_type,
        items_description: formData.items_description,
        quantity: formData.quantity || 1,
        condition: formData.condition,
        pickup_required: true,
        pickup_address: formData.pickup_address,
        pickup_city: formData.pickup_city,
        pickup_state: formData.pickup_state,
        pickup_zip: formData.pickup_zip,
        preferred_date: formData.preferred_date,
        preferred_time: formData.preferred_time,
        notes: formData.notes,
      };



      const response = await fetch(`${PUBLIC_API_URL}/donations`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(formData),
      });

      if (!response.ok) {
        throw new Error("Failed to schedule pickup");
      }

      formSuccess = true;

      // Reset form after success (commented out in the original, but useful)
      /* formData = {
        donor_name: "",
        donor_email: "",
        donor_phone: "",
        donation_type: "clothing",
        items_description: "",
        quantity: 1,
        condition: "good",
        pickup_required: true,
        pickup_address: "",
        pickup_city: "",
        pickup_state: "",
        pickup_zip: "",
        preferred_date: "",
        preferred_time: "",
        notes: "",
        terms: false,
      };
      */

      setTimeout(() => {
              window.location.href = "/";
     
      }, 3000);

    } catch (error) {
      console.error("Error submitting form:", error);
      formError = "Error connecting to the server or failed to schedule pickup.";
    } finally {
      isSubmitting = false;
    }
  };

  const toggleFaq = (faqId) => {
    openFaq = openFaq === faqId ? null : faqId;
  };

  const today = new Date();
  const minDate = new Date(today.getTime() + 2 * 24 * 60 * 60 * 1000).toISOString().split("T")[0];
</script>

<section
  id="pickup-service"
  class="py-16 md:py-24 bg-gray-50 dark:bg-neutral-900"
>
  <div class="container mx-auto px-4">
    <div class="max-w-4xl mx-auto">
      {#if formSuccess}
        <div
          class="mb-8 bg-green-100 dark:bg-green-900 border border-green-400 dark:border-green-700 text-green-700 dark:text-green-300 px-4 py-3 rounded relative"
          role="alert"
          in:slide
        >
          <strong class="font-bold">Success!</strong>
          <span class="block sm:inline">
            Your pickup has been scheduled. We'll send you a confirmation
            email shortly.
          </span>
        </div>
      {/if}

      {#if formError}
        <div
          class="mb-8 bg-red-100 dark:bg-red-900 border border-red-400 dark:border-red-700 text-red-700 dark:text-red-300 px-4 py-3 rounded relative"
          role="alert"
          in:slide
        >
          <strong class="font-bold">Error:</strong>
          <span class="block sm:inline"> {formError}</span>
        </div>
      {/if}

      <div class="text-center mb-16">
        <h2 class="text-3xl md:text-4xl font-bold text-gray-900 dark:text-white mb-4">
          Donation Pickup Service
        </h2>
        <p class="text-lg text-gray-600 dark:text-gray-300">
          Can't make it to a donation center? No problem! We offer a
          convenient pickup service to collect your donations directly from
          your doorstep.
        </p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-8 mb-16">
        <div class="bg-white dark:bg-neutral-800 rounded-xl shadow-md p-6 text-center transform transition duration-300 hover:shadow-lg hover:-translate-y-1">
          <div class="w-16 h-16 bg-green-100 dark:bg-green-900/30 rounded-full flex items-center justify-center mx-auto mb-4">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-8 w-8 text-green-600 dark:text-green-400"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
              />
            </svg>
          </div>
          <h3 class="text-xl font-semibold text-gray-900 dark:text-white mb-2">
            Schedule
          </h3>
          <p class="text-gray-600 dark:text-gray-300">
            Fill out our simple form to schedule a pickup at your preferred
            date and time.
          </p>
        </div>

        <div class="bg-white dark:bg-neutral-800 rounded-xl shadow-md p-6 text-center transform transition duration-300 hover:shadow-lg hover:-translate-y-1">
          <div class="w-16 h-16 bg-blue-100 dark:bg-blue-900/30 rounded-full flex items-center justify-center mx-auto mb-4">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-8 w-8 text-blue-600 dark:text-blue-400"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M5 8h14M5 8a2 2 0 110-4h14a2 2 0 110 4M5 8v10a2 2 0 002 2h10a2 2 0 002-2V8m-9 4h4"
              />
            </svg>
          </div>
          <h3 class="text-xl font-semibold text-gray-900 dark:text-white mb-2">
            Prepare
          </h3>
          <p class="text-gray-600 dark:text-gray-300">
            Pack your donations in bags or boxes and have them ready for
            collection.
          </p>
        </div>

        <div class="bg-white dark:bg-neutral-800 rounded-xl shadow-md p-6 text-center transform transition duration-300 hover:shadow-lg hover:-translate-y-1">
          <div class="w-16 h-16 bg-green-100 dark:bg-green-900/30 rounded-full flex items-center justify-center mx-auto mb-4">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-8 w-8 text-green-600 dark:text-green-400"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M5 13l4 4L19 7"
              />
            </svg>
          </div>
          <h3 class="text-xl font-semibold text-gray-900 dark:text-white mb-2">
            Relax
          </h3>
          <p class="text-gray-600 dark:text-gray-300">
            Our friendly drivers will come to your location and collect your
            donations.
          </p>
        </div>
      </div>

      <div class="bg-white dark:bg-neutral-800 rounded-xl shadow-lg overflow-hidden">
        <div class="md:flex">
          <div class="md:w-1/3 bg-green-600 p-8 text-white">
            <h3 class="text-2xl font-bold mb-4">Schedule a Pickup</h3>
            <p class="mb-6">
              Please provide your contact information and preferred pickup
              details.
            </p>
            <div class="space-y-4 text-sm">
              <div class="flex items-start">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 mr-3 mt-0.5"
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
                <div>
                  <strong class="block mb-1">
                    Available Pickup Times:
                  </strong>
                  <p>
                    Monday to Friday: 9am - 5pm
                    <br />
                    Saturday: 10am - 2pm
                  </p>
                </div>
              </div>
              <div class="flex items-start">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 mr-3 mt-0.5"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
                  />
                </svg>
                <div>
                  <strong class="block mb-1">Important Note:</strong>
                  <p>Please schedule at least 48 hours in advance.</p>
                </div>
              </div>
              <div class="flex items-start">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 mr-3 mt-0.5"
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
                <div>
                  <strong class="block mb-1">Questions?</strong>
                  <p>Call us at +91-123-4567</p>
                </div>
              </div>
            </div>
          </div>
          <div class="md:w-2/3 p-8">
            <form class="space-y-6" on:submit={handleSubmit}>
              <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div>
                  <label
                    for="donor_name"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    Full Name*
                  </label>
                  <input
                    type="text"
                    id="donor_name"
                    name="donor_name"
                    bind:value={formData.donor_name}
                    required
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  />
                </div>
                <div>
                  <label
                    for="donor_email"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    Email Address*
                  </label>
                  <input
                    type="email"
                    id="donor_email"
                    name="donor_email"
                    bind:value={formData.donor_email}
                    required
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  />
                </div>
              </div>

              <div>
                <label
                  for="donor_phone"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Phone Number
                </label>
                <input
                  type="tel"
                  id="donor_phone"
                  name="donor_phone"
                  bind:value={formData.donor_phone}
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                />
              </div>

              <div>
                <label
                  for="pickup_address"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Pickup Address*
                </label>
                <input
                  type="text"
                  id="pickup_address"
                  name="pickup_address"
                  bind:value={formData.pickup_address}
                  required
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                />
              </div>

              <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <div>
                  <label
                    for="pickup_city"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    City*
                  </label>
                  <input
                    type="text"
                    id="pickup_city"
                    name="pickup_city"
                    bind:value={formData.pickup_city}
                    required
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  />
                </div>
                <div>
                  <label
                    for="pickup_state"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    State*
                  </label>
                  <input
                    type="text"
                    id="pickup_state"
                    name="pickup_state"
                    bind:value={formData.pickup_state}
                    required
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  />
                </div>
                <div>
                  <label
                    for="pickup_zip"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    Zip Code*
                  </label>
                  <input
                    type="text"
                    id="pickup_zip"
                    name="pickup_zip"
                    bind:value={formData.pickup_zip}
                    required
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  />
                </div>
              </div>

              <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div>
                  <label
                    for="preferred_date"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    Preferred Pickup Date*
                  </label>
                  <input
                    type="date"
                    id="preferred_date"
                    name="preferred_date"
                    bind:value={formData.preferred_date}
                    required
                    min={minDate}
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  />
                </div>
                <div>
                  <label
                    for="preferred_time"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    Preferred Time*
                  </label>
                  <select
                    id="preferred_time"
                    name="preferred_time"
                    bind:value={formData.preferred_time}
                    required
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  >
                    <option value="">Select a time range</option>
                    <option value="morning">Morning (9am - 12pm)</option>
                    <option value="afternoon">Afternoon (12pm - 3pm)</option>
                    <option value="evening">Evening (3pm - 5pm)</option>
                  </select>
                </div>
              </div>

              <div>
                <label
                  for="donation_type"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Items You're Donating*
                </label>
                <select
                  id="donation_type"
                  name="donation_type"
                  multiple
                  on:change={handleMultiSelect}
                  required
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  size="4"
                >
                  <option value="clothing">Clothing</option>
                  <option value="accessories">Accessories</option>
                  <option value="household">Household Items</option>
                  <option value="other">Other</option>
                </select>
                <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">
                  Hold Ctrl/Cmd to select multiple items
                </p>
              </div>

              <div>
                <label
                  for="items_description"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Brief Description of Items*
                </label>
                <textarea
                  id="items_description"
                  name="items_description"
                  rows="2"
                  bind:value={formData.items_description}
                  required
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  placeholder="E.g., Winter clothes, kitchenware, books"
                ></textarea>
              </div>

              <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div>
                  <label
                    for="quantity"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    Approximate Quantity
                  </label>
                  <select

                    id="quantity"
                    name="quantity"
                    bind:value={formData.quantity}
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  >
                    <option value="1">Small (1-2 bags/boxes)</option>
                    <option value="3">Medium (3-5 bags/boxes)</option>
                    <option value="8">Large (6-10 bags/boxes)</option>
                    <option value="15">Extra Large (10+ bags/boxes)</option>
                  </select>
                </div>
                <div>
                  <label
                    for="condition"
                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                  >
                    Condition
                  </label>
                  <select
                    id="condition"
                    name="condition"
                    bind:value={formData.condition}
                    class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  >
                    <option value="new">New</option>
                    <option value="like_new">Like New</option>
                    <option value="good">Good</option>
                    <option value="fair">Fair</option>
                    <option value="poor">Poor</option>
                  </select>
                </div>
              </div>

              <div>
                <label
                  for="notes"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Special Instructions
                </label>
                <textarea
                  id="notes"
                  name="notes"
                  rows="3"
                  bind:value={formData.notes}
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-700 text-gray-900 dark:text-white"
                  placeholder="Parking information, gate code, specific item details, etc."
                ></textarea>
              </div>

              <div class="flex items-start">
                <div class="flex items-center h-5">
                  <input
                    id="terms"
                    name="terms"
                    type="checkbox"
                    bind:checked={formData.terms}
                    class="h-4 w-4 text-green-600 focus:ring-green-500 border-gray-300 rounded"
                  />
                </div>
                <div class="ml-3 text-sm">
                  <label
                    for="terms"
                    class="font-medium text-gray-700 dark:text-gray-300"
                  >
                    I agree to the pickup
                    <a
                      href="#"
                      class="text-green-600 hover:text-green-500"
                    >
                      terms and conditions
                    </a>
                  </label>
                </div>
              </div>

              <div>
                <button
                  type="submit"
                  disabled={isSubmitting}
                  class={`w-full py-3 px-4 ${isSubmitting ? "bg-gray-400 cursor-not-allowed" : "bg-green-600 hover:bg-green-700"} text-white font-medium rounded-lg transition duration-300 transform hover:scale-105`}
                >
                  {#if isSubmitting}
                    Processing...
                  {:else}
                    Schedule Pickup
                  {/if}
                </button>
              </div>
            </form>

            {#if formSuccess}
              <div
                class="mb-8 mt-8 bg-green-100 dark:bg-green-900 border border-green-400 dark:border-green-700 text-green-700 dark:text-green-300 px-4 py-3 rounded relative"
                role="alert"
                in:slide
              >
                <strong class="font-bold">Success!</strong>
                <span class="block sm:inline">
                  Your pickup has been scheduled. We'll send you a
                  confirmation email shortly.
                </span>
              </div>
            {/if}

            {#if formError}
              <div
                class="mb-8 bg-red-100 dark:bg-red-900 border border-red-400 dark:border-red-700 text-red-700 dark:text-red-300 px-4 py-3 rounded relative"
                role="alert"
                in:slide
              >
                <strong class="font-bold">Error:</strong>
                <span class="block sm:inline"> {formError}</span>
              </div>
            {/if}
          </div>
        </div>
      </div>

      <div class="mt-16">
        <h3 class="text-2xl font-bold text-gray-900 dark:text-white text-center mb-8">
          Frequently Asked Questions
        </h3>

        <div class="space-y-4">
          <div class="bg-white dark:bg-neutral-800 rounded-lg shadow-md overflow-hidden">
            <button
              on:click={() => toggleFaq(1)}
              class="flex justify-between items-center w-full px-6 py-4 text-left font-medium text-gray-900 dark:text-white"
            >
              <span>How far in advance should I schedule a pickup?</span>
              <svg
                class={`h-5 w-5 text-gray-500 dark:text-gray-400 transform transition-transform ${openFaq === 1 ? "rotate-180" : ""}`}
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="currentColor"
              >
                <path
                  fill-rule="evenodd"
                  d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                  clip-rule="evenodd"
                />
              </svg>
            </button>
            {#if openFaq === 1}
              <div
                class="px-6 pb-4 text-gray-600 dark:text-gray-300"
                transition:slide
              >
                <p>
                  We recommend scheduling your pickup at least 48 hours in
                  advance to ensure we can accommodate your preferred date and
                  time. During busy periods, especially around holidays, it's
                  best to schedule a week in advance.
                </p>
              </div>
            {/if}
          </div>

          <div class="bg-white dark:bg-neutral-800 rounded-lg shadow-md overflow-hidden">
            <button
              on:click={() => toggleFaq(2)}
              class="flex justify-between items-center w-full px-6 py-4 text-left font-medium text-gray-900 dark:text-white"
            >
              <span>Is there a fee for pickup service?</span>
              <svg
                class={`h-5 w-5 text-gray-500 dark:text-gray-400 transform transition-transform ${openFaq === 2 ? "rotate-180" : ""}`}
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="currentColor"
              >
                <path
                  fill-rule="evenodd"
                  d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                  clip-rule="evenodd"
                />
              </svg>
            </button>
            {#if openFaq === 2}
              <div
                class="px-6 pb-4 text-gray-600 dark:text-gray-300"
                transition:slide
              >
                <p>
                  Our pickup service is completely free! We offer this service
                  to make donating as convenient as possible for our generous
                  supporters.
                </p>
              </div>
            {/if}
          </div>

          <div class="bg-white dark:bg-neutral-800 rounded-lg shadow-md overflow-hidden">
            <button
              on:click={() => toggleFaq(3)}
              class="flex justify-between items-center w-full px-6 py-4 text-left font-medium text-gray-900 dark:text-white"
            >
              <span>Do I need to be home during the pickup?</span>
              <svg
                class={`h-5 w-5 text-gray-500 dark:text-gray-400 transform transition-transform ${openFaq === 3 ? "rotate-180" : ""}`}
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="currentColor"
              >
                <path
                  fill-rule="evenodd"
                  d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                  clip-rule="evenodd"
                />
              </svg>
            </button>
            {#if openFaq === 3}
              <div
                class="px-6 pb-4 text-gray-600 dark:text-gray-300"
                transition:slide
              >
                <p>
                  We recommend that someone be present during the pickup to
                  ensure everything goes smoothly. However, if you're unable
                  to be home, we can make arrangements for your items to be
                  left in a secure, weather-protected area. Please provide
                  detailed instructions in the "Special Instructions" section
                  of the form.
                </p>
              </div>
            {/if}
          </div>

          <div class="bg-white dark:bg-neutral-800 rounded-lg shadow-md overflow-hidden">
            <button
              on:click={() => toggleFaq(4)}
              class="flex justify-between items-center w-full px-6 py-4 text-left font-medium text-gray-900 dark:text-white"
            >
              <span>How do I prepare my items for pickup?</span>
              <svg
                class={`h-5 w-5 text-gray-500 dark:text-gray-400 transform transition-transform ${openFaq === 4 ? "rotate-180" : ""}`}
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="currentColor"
              >
                <path
                  fill-rule="evenodd"
                  d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                  clip-rule="evenodd"
                />
              </svg>
            </button>
            {#if openFaq === 4}
              <div
                class="px-6 pb-4 text-gray-600 dark:text-gray-300"
                transition:slide
              >
                <p>
                  Please pack your items in sturdy bags or boxes that are not
                  too heavy for one person to lift. If possible, sort items by
                  category (clothing, household, etc.) and ensure all items
                  are clean and in good condition. For fragile items, please
                  mark the box as "FRAGILE." Having your donations ready and
                  accessible makes the pickup process quick and efficient.
                </p>
              </div>
            {/if}
          </div>

          <div class="bg-white dark:bg-neutral-800 rounded-lg shadow-md overflow-hidden">
            <button
              on:click={() => toggleFaq(5)}
              class="flex justify-between items-center w-full px-6 py-4 text-left font-medium text-gray-900 dark:text-white"
            >
              <span>What happens if I need to reschedule my pickup?</span>
              <svg
                class={`h-5 w-5 text-gray-500 dark:text-gray-400 transform transition-transform ${openFaq === 5 ? "rotate-180" : ""}`}
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="currentColor"
              >
                <path
                  fill-rule="evenodd"
                  d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                  clip-rule="evenodd"
                />
              </svg>
            </button>
            {#if openFaq === 5}
              <div
                class="px-6 pb-4 text-gray-600 dark:text-gray-300"
                transition:slide
              >
                <p>
                  If you need to reschedule your pickup, please contact us at
                  least 24 hours in advance at (555) 123-4567. We understand
                  that circumstances change, and we'll do our best to
                  accommodate your new preferred date and time.
                </p>
              </div>
            {/if}
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
