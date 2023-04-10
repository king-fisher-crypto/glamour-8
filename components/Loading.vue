<template>
	<div class="loading animate-fadeIn">
		<Logo />
		<div class="image-wrapper d-flex just-center">
				<img src="../assets/images/loading.png" class="main-image" alt="main-image">
		</div>
		<h1 class="month">MAREC</h1>
	</div>
</template>
	
<script>
export default {
	name: 'Loading',
	mounted() {
		this.init()
	},
	methods: {
		init() {
			setTimeout(this.touch, 3000)
		},
		touch() {
			document.querySelector('.loading .main-image').style.transform = 'rotate(-90deg)'
			document.querySelector('.loading .main-image').style.width = '195px'
			document.querySelector('.loading .main-image').style.marginTop = '-52px'
			document.querySelector('.loading .main-image').style.marginLeft = '15px'

			setTimeout(() => {
				document.querySelector('.loading').style.display = 'none'
				document.querySelector('.setup').style.display = 'block'
			}, 500);
		}
	}
};
</script>