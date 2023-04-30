<script lang="ts">
	import { io } from 'socket.io-client';
	import { onMount } from 'svelte';

	let message = '';
	let messages: string[] = [];
	let socket: any;

	onMount(() => {
		socket = io('http://localhost:3000/');

		socket.on('chat message', function (msg: string) {
			messages.push(msg);
			messages = messages;
		});

		socket.on('load all', function (messagesDB: string[]) {
			messagesDB.forEach((msg: string) => {
				messages.push(msg);
				messages = messages;
			});
		});
	});

	function submit() {
		if (message) {
			socket.emit('chat message', message);
			message = '';
		}
	}
</script>

<svelte:head>
	<title>Chat</title>
	<meta name="description" content="Chat" />
</svelte:head>

<div>
	<ul id="messages">
		{#each messages as message, i}
			<li>
				{message}
			</li>
		{/each}
	</ul>
	<form id="form" action="">
		<input bind:value={message} id="input" autocomplete="off" />
		<button type="button" on:click={submit}>Enviar</button>
	</form>
</div>

<style>
	body {
		margin: 0;
		padding-bottom: 3rem;
		font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
	}

	#form {
		background: rgba(0, 0, 0, 0.15);
		padding: 0.25rem;
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		display: flex;
		height: 3rem;
		box-sizing: border-box;
		backdrop-filter: blur(10px);
	}
	#input {
		border: none;
		padding: 0 1rem;
		flex-grow: 1;
		border-radius: 2rem;
		margin: 0.25rem;
	}
	#input:focus {
		outline: none;
	}
	#form > button {
		background: #333;
		border: none;
		padding: 0 1rem;
		margin: 0.25rem;
		border-radius: 3px;
		outline: none;
		color: #fff;
	}

	#messages {
		list-style-type: none;
		margin: 0;
		padding: 0;
	}
	#messages > li {
		padding: 0.5rem 1rem;
	}
	#messages > li:nth-child(odd) {
		background: #efefef;
	}
</style>
