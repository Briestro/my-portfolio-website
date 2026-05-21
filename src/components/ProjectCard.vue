<template>
	<div class="col-md-6 col-lg-4">
	    <div class="card project-card h-100">
	        <img :src="project.image" class="card-img-top" loading="lazy" :alt="project.alt">
	        <div class="card-body">
	            <h5 class="card-title">{{ project.title }}</h5>
	            <p class="card-text">{{ project.description }}</p>
	        </div>
	    </div>
	</div>
</template>

<script setup>
	import { defineProps } from 'vue'

	// Props passed in from Projects.vue via v-for
	// Sample project object:
	// {
	//   "id": 1,
	//   "title": "E-commerce API Documentation",
	//   "description": "Dynamic E-Commerce Workflow Management...",
	//   "image": "/images/eAPI.png",
	//   "alt": "E-commerce API Documentation"
	// }
	defineProps({
		project: Object
	})
</script>
