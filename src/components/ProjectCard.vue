<script setup>

    defineProps({
        project: Object
    })
</script>

<template>
	
	<div class="project-box">
		<div class="card p-3">
		  <img :src="project.image" class="card-img-top" alt="Project">
		  <div class="card-body">
		    <h5 class="card-title text-white">{{project.title}}</h5>
		    <p class="card-text">{{project.description}}</p>

		    <a :href="project.link" target="_blank" class="btn btn-primary">View Project</a>
		  </div>
		</div>
	</div>

</template>