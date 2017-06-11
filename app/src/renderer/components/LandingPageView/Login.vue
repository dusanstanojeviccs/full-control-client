<template>
	<div class="login-page">
		<div class="form">
			<div class="logo-icon"></div>
			<div class="error" v-if="error">{{error}}</div>
			<form class="login-form" v-on:submit.prevent="submit">
				<input v-model="username" type="text" placeholder="username">
				<input v-model="password" type="password" placeholder="password">
				<button>login</button>
			</form>
		</div>
	</div>
</template>

<script>
	export default {
		props: ['error'],
		data: function() {
			return {
				username: '',
				password: ''
			}
		},
		methods: {
			submit: function() {
				this.$emit('submit', this.username, this.password);
			}
		}
	}
</script>


<style scoped>
	.login-card img {
		max-width: 150px;
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
	.login-form {
		margin-top: 30px;
	}
	.login-page {
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
	.form input {
		font-family: "Roboto", sans-serif;
		outline: 0;
		background: #f2f2f2;
		width: 100%;
		border: 0;
		margin: 0 0 15px;
		padding: 15px;
		box-sizing: border-box;
		font-size: 14px;
	}
	.form button {
		font-family: "Roboto", sans-serif;
		text-transform: uppercase;
		outline: 0;
		background: #1b96fe;
		width: 100%;
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
	.form .message {
		margin: 15px 0 0;
		color: #b3b3b3;
		font-size: 12px;
	}
	.form .message a {
		color: #03A9F4;
		text-decoration: none;
	}
	.form .register-form {
		display: none;
	}
	.container {
		position: relative;
		z-index: 1;
		max-width: 300px;
		margin: 0 auto;
	}
	.container:before, .container:after {
		content: "";
		display: block;
		clear: both;
	}
	.container .info {
		margin: 50px auto;
		text-align: center;
	}
	.container .info h1 {
		margin: 0 0 15px;
		padding: 0;
		font-size: 36px;
		font-weight: 300;
		color: #1a1a1a;
	}
	.container .info span {
		color: #4d4d4d;
		font-size: 12px;
	}
	.container .info span a {
		color: #000000;
		text-decoration: none;
	}
	.container .info span .fa {
		color: #EF3B3A;
	}
	.error {
		background: rgb(230, 1, 43);
		color: white;
		padding: 7px;
		margin-top: 25px;
		font-size: 14px;
		margin-bottom: -15px;
	}
</style>
