<script setup>
	import { computed } from 'vue';
	import ProjectCard from './ProjectCard.vue';
	import projects from '../data/projects.json';

	const chunkSize = 3;

	const chunkProjects = computed(() => {
		const chunks = [];

		for (let i = 0; i < projects.length; i+= chunkSize) {
			chunks.push(projects.slice(i, i + chunkSize));
		}

		return chunks;
	})
</script>

<template>
	
	<section id="projects" class="pb-5">
			<div class="container-xl pb-5 blue-divider">
				<h2 class="text-white fw-bold">My Projects</h2>

				<div class="py-4 d-flex flex-wrap justify-content-start gap-4 align-items-stretch" v-for="(group, index) in chunkProjects" :key="index">

					<ProjectCard v-for="project in group" :key="project.id" :project="project" />
					
				</div>

			</div>

		</section>


</template>