<template>
	<div class="status-page">
		<div class="form">
			<div class="logo-icon"></div>

			<div v-if="disconnected">
				<div class="status red">Status: Disconnected</div>
				<div class="button reconnect" v-on:click="reconnect">Reconnect</div>
			</div>

			<div v-else>
				<div class="status green">Status: {{status}}</div>
				<div class="button logout" v-on:click="logout">Logout</div>
			</div>
			
		</div>
	</div>
</template>

<script>
	export default {
		props: ['status', 'disconnected'],
		methods: {
			reconnect: function() {
				this.$emit('reconnect');
			},
			logout: function() {
				this.$emit('logout');
			}
		}
	}
</script>

<style scoped>
	.red {
		color: rgb(230, 1, 43);
	}
	.green {
		color: #58ca29;
	}
	.logo-icon {
		background-image: url("./assets/logo.png");
		margin: 0;
		width: 150px;
		height: 54px;
		top: 12px;
		background-size: 150px auto; 
		background-repeat: no-repeat; 
		background-position: center;
		margin: 10px auto;
	}
	.status-page {
		margin-top: 30px;
	}
	.status-page {
		width: 360px;
		padding: 8% 0 0;
		margin: auto;
	}
	.form {
		position: relative;
		z-index: 1;
		background: #FFFFFF;
		max-width: 360px;
		margin: 0 auto 100px;
		padding: 45px;
		text-align: center;
	}
	.form .button {
		font-family: "Roboto", sans-serif;
		text-transform: uppercase;
		outline: 0;
		background: #1b96fe;
		border: 0;
		padding: 15px;
		color: #FFFFFF;
		font-size: 14px;
		-webkit-transition: all 0.3 ease;
		transition: all 0.3 ease;
		cursor: pointer;
	}
	.form button:hover,.form button:active,.form button:focus {
		background: #44a7fb;
	}
	.status {
		margin-top: 60px;
		margin-bottom: 40px;
	}
</style>
