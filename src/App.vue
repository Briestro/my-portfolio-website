<script setup>
	import Navbar from './components/Navbar.vue'
	import About from './components/About.vue'
	import Projects from './components/Projects.vue'
	import Tools from './components/Tools.vue'
	import Contact from './components/Contact.vue'
	import FooterComponent from './components/FooterComponent.vue'
	import { onMounted } from 'vue'

	// Back to top button — migrated from original inline <script> in index.html
	onMounted(() => {
		const backToTopButton = document.querySelector('.back-to-top')

		window.addEventListener('scroll', () => {
			if (window.scrollY > 300) {
				backToTopButton.classList.add('show')
			} else {
				backToTopButton.classList.remove('show')
			}
		})
	})
</script>

<template>
	<main>
		<Navbar />
		<About />
		<Projects />
		<Tools />
		<Contact />
		<FooterComponent />

		<a href="#" class="back-to-top">
			<i class="fas fa-arrow-up"></i>
		</a>
	</main>
</template>
