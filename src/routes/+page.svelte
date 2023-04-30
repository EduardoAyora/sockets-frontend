<script lang="ts">
	import { io } from 'socket.io-client';

	let message = '';
	let messages: string[] = [];
	let socket: any;
	let url = 'http://192.168.1.9:3000';

	function connectSocket() {
		socket = io(url);

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

		alert(`Conectado a ${url}`);
	}

	function submit() {
		if (!socket) alert('No está conectado');
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
		<input bind:value={url} id="input" autocomplete="off" />
		<button type="button" on:click={connectSocket}>Conectar</button>
	</form>
</div>

<style>
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
