<template>
	<!-- project section start -->
	<section id="projects" class="py-100">
	    <div class="container">
	        <h2 class="section-title text-center mb-5">My Projects</h2>
	        
	        <!-- Using the same chunkedProjects method from the discussion project:
	             splits the projects array into groups of 3, renders each group as a row -->
	        <div class="row g-4">
	        	<ProjectCard
	        		v-for="project in projects"
	        		:key="project.id"
	        		:project="project"
	        	/>
	        </div>
	    </div>
	</section>
	<!-- Project section end -->
</template>

<script setup>
	import { computed } from 'vue'
	import ProjectCard from './ProjectCard.vue'
	import projectsData from '../data/projects.json'

	const chunkSize = 3

	// Splits the projects array into chunks of 3 — same method as the discussion project
	const chunkedProjects = computed(() => {
		const chunks = []
		for (let i = 0; i < projectsData.length; i += chunkSize) {
			chunks.push(projectsData.slice(i, i + chunkSize))
		}
		return chunks
	})

	// Flat list used for rendering — the original HTML uses a single .row g-4
	// with individual col-md-6 col-lg-4 cards, which ProjectCard handles internally
	const projects = projectsData
</script>
