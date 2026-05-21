<script setup>
	import { ref, onMounted, onBeforeUnmount } from 'vue';

	import { Notyf } from 'notyf';
	import 'notyf/notyf.min.css';

	const notyf = new Notyf();

	const WEB3FORMS_ACCESS_KEY = "931fa0b5-c693-4a72-8282-5f8742347a6c";

	const subject = "New message from Portfolio Contact Form";

	// v-model bindings for each form field
	const name = ref("");
	const email = ref("");
	const message = ref("");

	const isLoading = ref(false);

	const submitForm = async () => {

		if (!recaptchaToken.value) {
			notyf.error('Please verify that you are not a robot.');
			return;
		}

		isLoading.value = true;

		try {
			const response = await fetch("https://api.web3forms.com/submit", {
				method: "POST",
				headers: {
					"Content-Type": "application/json",
					Accept: "application/json"
				},
				body: JSON.stringify({
					access_key: WEB3FORMS_ACCESS_KEY,
					subject: subject,
					name: name.value,
					email: email.value,
					message: message.value
				})
			});

			const result = await response.json();

			if (result.success) {
				// Clear form fields on success
				name.value = "";
				email.value = "";
				message.value = "";

				isLoading.value = false;
				notyf.success("Message sent!");
			} else {
				throw new Error("Submission failed");
			}

		} catch (error) {
			console.log(error);
			isLoading.value = false;
			notyf.error("Failed to send message. Please try again.");

		} finally {
			resetRecaptcha();
		}
	}


	const SITE_KEY = '6LcoKvUsAAAAAEUJU9-xdV38t_eqk7rQ08efv3vg';

	const recaptchaContainer = ref(null);
	const recaptchaWidgetId = ref(null);
	const recaptchaToken = ref('');

	// Called by reCAPTCHA when the user checks the box
	function onRecaptchaSuccess(token) {
		recaptchaToken.value = token;
	}

	function onRecaptchaExpired() {
		recaptchaToken.value = '';
	}

	function renderRecaptcha() {
		if (!window.grecaptcha) {
			console.error('reCAPTCHA not loaded');
			return;
		}

		recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
			sitekey: SITE_KEY,
			size: 'normal',
			callback: onRecaptchaSuccess,
			'expired-callback': onRecaptchaExpired
		});
	}

	function resetRecaptcha() {
		if (recaptchaWidgetId.value !== null) {
			window.grecaptcha.reset(recaptchaWidgetId.value);
			recaptchaToken.value = '';
		}
	}

	onMounted(() => {
		const interval = setInterval(() => {
			if (window.grecaptcha && window.grecaptcha.render) {
				renderRecaptcha();
				clearInterval(interval);
			}
		}, 100);

		onBeforeUnmount(() => {
			clearInterval(interval);
		});
	});
</script>

<template>
	<!-- contact section start -->
	<section id="contact" class="py-100">
	    <div class="container">
	        <h2 class="section-title text-center mb-5">Contact Me</h2>
	        
	        <div class="row g-5 align-items-stretch">
	        	<div class="col-lg-5">
	                <div class="contact-info-card p-4 h-100">
	                    <h3 class="mb-4">Get In Touch</h3>
	                    <p class="mb-4 text-muted">Feel free to reach out for collaborations or project inquiries.</p>
	                    <ul class="list-unstyled">
	                        <li class="mb-3"><i class="fas fa-envelope me-2 text-cyan"></i> briankellyroque@gmail.com</li>
	                        <li class="mb-3"><i class="fas fa-phone me-2 text-cyan"></i> +63 919 967 6175</li>
	                        <li class="mb-3"><i class="fas fa-map-marker-alt me-2 text-cyan"></i> Metro Manila, PH</li>
	                    </ul>
	                </div>
	            </div>

	            <div class="col-lg-7">
	            	<!-- @submit.prevent stops page reload; calls submitForm instead -->
	                <form id="contact-form" class="p-4 rounded h-100" @submit.prevent="submitForm">
	                    <div class="mb-3">
	                        <label for="user-name" class="form-label">Full Name</label>
	                        <input
	                        	type="text"
	                        	v-model="name"
	                        	class="form-control"
	                        	id="user-name"
	                        	placeholder="Your Name"
	                        	required
	                        >
	                    </div>
	                    <div class="mb-3">
	                        <label for="user-email" class="form-label">Email address</label>
	                        <input
	                        	type="email"
	                        	v-model="email"
	                        	class="form-control"
	                        	id="user-email"
	                        	placeholder="name@example.com"
	                        	required
	                        >
	                    </div>
	                    <div class="mb-3">
	                        <label for="user-message" class="form-label">Message</label>
	                        <textarea
	                        	v-model="message"
	                        	class="form-control"
	                        	id="user-message"
	                        	rows="5"
	                        	placeholder="How can I help you?"
	                        	required
	                        ></textarea>
	                    </div>

	                    <!-- reCAPTCHA widget renders here via grecaptcha.render() in onMounted -->
	                    <div class="d-flex justify-content-end mb-3">
	                    	<div ref="recaptchaContainer"></div>
	                    </div>
	                    
	                    <div class="d-flex justify-content-between align-items-center mt-4">
	                        <div class="social-links">
	                            <a href="https://www.linkedin.com/in/brian-kelly-roque-411853254/" target="_blank" class="text-white me-3 fs-4">
	                            	<i class="fab fa-linkedin" aria-label="LinkedIn Profile"></i>
	                            </a>
	                            <a href="https://github.com/Briestro" target="_blank" class="text-white fs-4">
	                            	<i class="fab fa-github" aria-label="Github Profile"></i>
	                            </a>
	                        </div>
	                        <!-- :disabled binds to isLoading so button is disabled while sending -->
	                        <button type="submit" class="btn btn-cyan px-5 py-2" :disabled="isLoading">
	                        	{{ isLoading ? "Sending..." : "Send Message" }}
	                        </button>
	                    </div>
	                </form>
	            </div>
	        </div>
	    </div>
	</section>
	<!-- end of contact section -->
</template>
