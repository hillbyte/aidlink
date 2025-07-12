<script>
	import Header from '$lib/components/Header.svelte';
	import Hero from '$lib/components/Hero.svelte';
	import HowItWorks from '$lib/components/HowItWorks.svelte';
	import DonationItems from '$lib/components/DonationItems.svelte';
	import ImpactStories from '$lib/components/ImpactStories.svelte';
	import DonationCenters from '$lib/components/DonationCenters.svelte';
	import PickupService from '$lib/components/PickupService.svelte';
	import Volunteer from '$lib/components/Volunteer.svelte';
	import Contact from '$lib/components/Contact.svelte';
	import Footer from '$lib/components/Footer.svelte';
</script>

<Header />
<main>
	<Hero />
	<HowItWorks />
	<DonationItems />
	<DonationCenters />
	<PickupService />
	<Volunteer />
	<Contact />
</main>
<Footer />
