<script>
  import { fade } from 'svelte/transition';
import {
PUBLIC_API_URL
	} from '$env/static/public';
  let formData = {
    name: '',
    email: '',
    phone: '',
    subject: '',
    message: ''
  };

  let isSubmitting = false;
  let submitStatus = null; 

  const handleChange = (event) => {
    const { name, value } = event.target;
    formData = { ...formData, [name]: value };
  };

  const handleSubmit = async (event) => {
    event.preventDefault();
    isSubmitting = true;
    submitStatus = null;

    try {
      await fetch(`${PUBLIC_API_URL}/contact`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(formData),
      });

      console.log('Form Submitted:', formData);
      submitStatus = { 
        success: true, 
        message: 'Thank you for your message! We will get back to you soon.' 
      };
      
      formData = {
        name: '',
        email: '',
        phone: '',
        subject: '',
        message: ''
      };
      
    } catch (error) {
      submitStatus = { 
        success: false, 
        message: 'An error occurred. Please try again later.' 
      };
    } finally {
      isSubmitting = false;
    }
  };
</script>

<section
  id="contact"
  class="py-16 md:py-24 bg-white dark:bg-neutral-800"
>
  <div class="container mx-auto px-4">
    <div class="max-w-4xl mx-auto">
      <div class="text-center mb-16">
        <h2 class="text-3xl md:text-4xl font-bold text-gray-900 dark:text-white mb-4">
          Contact Us
        </h2>
        <p class="text-lg text-gray-600 dark:text-gray-300">
          We'd love to hear from you! Whether you have a question, a
          suggestion, or need assistance, please don't hesitate to reach
          out.
        </p>
      </div>

      <div class="bg-gray-50 dark:bg-neutral-700 rounded-xl shadow-md overflow-hidden">
        <div class="md:flex">
          <div class="md:w-1/2 p-8">
            <h3 class="text-2xl font-bold text-gray-900 dark:text-white mb-6">
              Send Us a Message
            </h3>

            {#if submitStatus}
              <div
                in:fade={{ duration: 300 }}
                class={`mb-6 p-4 rounded-lg ${submitStatus.success ? "bg-green-100 text-green-800 dark:bg-green-800 dark:text-green-100" : "bg-red-100 text-red-800 dark:bg-red-800 dark:text-red-100"}`}
              >
                {submitStatus.message}
              </div>
            {/if}

            <form class="space-y-6" on:submit={handleSubmit}>
              <div>
                <label
                  for="name"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Full Name
                </label>
                <input
                  type="text"
                  id="name"
                  name="name"
                  bind:value={formData.name}
                  required
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-800 text-gray-900 dark:text-white"
                />
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
                  bind:value={formData.email}
                  required
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-800 text-gray-900 dark:text-white"
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
                  type="text"
                  id="phone"
                  name="phone"
                  bind:value={formData.phone}
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-800 text-gray-900 dark:text-white"
                />
              </div>
              <div>
                <label
                  for="subject"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Subject
                </label>
                <input
                  type="text"
                  id="subject"
                  name="subject"
                  bind:value={formData.subject}
                  required
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-800 text-gray-900 dark:text-white"
                />
              </div>
              <div>
                <label
                  for="message"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                >
                  Message
                </label>
                <textarea
                  id="message"
                  name="message"
                  rows="4"
                  bind:value={formData.message}
                  required
                  class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 bg-white dark:bg-neutral-800 text-gray-900 dark:text-white"
                  placeholder="Your message..."
                ></textarea>
              </div>
              <div>
                <button
                  type="submit"
                  disabled={isSubmitting}
                  class="w-full py-3 px-4 bg-green-600 hover:bg-green-700 text-white font-medium rounded-lg transition duration-300 transform hover:scale-105 disabled:opacity-70 disabled:cursor-not-allowed"
                >
                  {#if isSubmitting}
                    Sending...
                  {:else}
                    Send Message
                  {/if}
                </button>
              </div>
            </form>
          </div>
          <div class="md:w-1/2 bg-green-600 p-8 text-white">
            <h3 class="text-2xl font-bold mb-6">Contact Information</h3>
            <div class="space-y-6">
              <div class="flex items-start">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-6 w-6 mr-4 mt-1"
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
                <div>
                  <h4 class="font-semibold">Our Address</h4>
                  <p>123 Street, Downtown, Kolkata 733105</p>
                </div>
              </div>
              <div class="flex items-start">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-6 w-6 mr-4 mt-1"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
                  />
                </svg>
                <div>
                  <h4 class="font-semibold">Email Us</h4>
                  <p>contact@aidlink.org</p>
                </div>
              </div>
              <div class="flex items-start">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-6 w-6 mr-4 mt-1"
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
                  <h4 class="font-semibold">Call Us</h4>
                  <p>+91-123-4567</p>
                </div>
              </div>
            </div>
            <div class="mt-8">
              <h4 class="font-semibold mb-4">Follow Us</h4>
              <div class="flex space-x-4">
                <a
                  href="#"
                  class="text-white hover:text-gray-200 transition duration-300"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="h-6 w-6"
                    fill="currentColor"
                    viewBox="0 0 24 24"
                  >
                    <path d="M22.675 0h-21.35c-.732 0-1.325.593-1.325 1.325v21.351c0 .731.593 1.324 1.325 1.324h11.495v-9.294h-3.128v-3.622h3.128v-2.671c0-3.1 1.893-4.788 4.659-4.788 1.325 0 2.463.099 2.795.143v3.24l-1.918.001c-1.504 0-1.795.715-1.795 1.763v2.313h3.587l-.467 3.622h-3.12v9.293h6.116c.73 0 1.323-.593 1.323-1.325v-21.35c0-.732-.593-1.325-1.323-1.325z" />
                  </svg>
                </a>
                <a
                  href="#"
                  class="text-white hover:text-gray-200 transition duration-300"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="h-6 w-6"
                    fill="currentColor"
                    viewBox="0 0 24 24"
                  >
                    <path d="M24 4.557c-.883.392-1.832.656-2.828.775 1.017-.609 1.798-1.574 2.165-2.724-.951.564-2.005.974-3.127 1.195-.897-.957-2.178-1.555-3.594-1.555-3.179 0-5.515 2.966-4.797 6.045-4.091-.205-7.719-2.165-10.148-5.144-1.29 2.213-.669 5.108 1.523 6.574-.806-.026-1.566-.247-2.229-.616v.064c0 2.298 1.634 4.212 3.796 4.649-.69.188-1.43.233-2.18.084.624 1.955 2.44 3.379 4.6 3.419-2.07 1.623-4.678 2.588-7.52 2.588-.49 0-.974-.028-1.455-.086 2.685 1.726 5.875 2.73 9.28 2.73 10.59 0 16.375-8.775 16.375-16.375 0-.25-.005-.499-.015-.747.995-.718 1.858-1.62 2.548-2.644z" />
                  </svg>
                </a>
                <a
                  href="#"
                  class="text-white hover:text-gray-200 transition duration-300"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="h-6 w-6"
                    fill="currentColor"
                    viewBox="0 0 24 24"
                  >
                    <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.85s-.012 3.584-.07 4.85c-.148 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07s-3.584-.012-4.85-.07c-3.252-.148-4.771-1.691-4.919-4.919-.058-1.265-.07-1.645-.07-4.85s.012-3.584.07-4.85c.148-3.225 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.85-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948s.014 3.667.072 4.947c.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072s3.667-.014 4.947-.072c4.358-.2 6.78-2.618 6.98-6.98.059-1.281.073-1.689.073-4.948s-.014-3.667-.072-4.947c-.2-4.358-2.618-6.78-6.98-6.98-1.281-.059-1.689-.073-4.948-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.162 6.162 6.162 6.162-2.759 6.162-6.162-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4s1.791-4 4-4 4 1.79 4 4-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44 1.441-.645 1.441-1.44-.645-1.44-1.441-1.44z" />
                  </svg>
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
