<script lang="ts">
	import { app } from '@neutralinojs/lib';
	import { SettingsPanelBuilder } from '../components/settings';
	import Panel from '../components/settings/panel.svelte';

	export let render = true;

	async function onSwitchClicked(e: CustomEvent) {
		const { id, state } = e.detail;
		switch (id) {
			case 'window':
				if (!state) return;
				await app.writeProcessOutput('askPerm');
				app.readProcessInput();
				break;
		}
	}

	const panel = new SettingsPanelBuilder()
		.setName('Integrations')
		.setDescription('Configure AppleBlox integrations with Discord and Roblox')
		.setId('integrations')
		.addCategory((category) =>
			category
				.setName('Game Activity')
				.setDescription('In-game information display')
				.setId('activity')
				.addSwitch({
					label: 'Server Location',
					description: "Display server's geographic location when joining games",
					id: 'notify_location',
					default: true,
				})
		)
		.addCategory((category) =>
			category
				.setName('Bloxstrap SDK')
				.setDescription('In-house implementation of Bloxstrap SDK for game developers')
				.setId('sdk')
				.addSwitch({
					label: 'Enable SDK',
					description: 'Enable Bloxstrap SDK compatibility features',
					id: 'enabled',
					default: false,
				})
				.addSwitch({
					label: 'Discord RPC Control',
					description: 'Allow games to customize your Discord presence',
					id: 'rpc',
					default: false,
					toggleable: {
						id: 'enabled',
						type: 'switch',
						value: true,
					},
				})
		)
		.addCategory((category) =>
			category
				.setName('Discord Rich Presence')
				.setDescription('Show your Roblox activity on Discord')
				.setId('rpc')
				.addSwitch({
					label: 'Enable Rich Presence',
					description: 'Display your game activity on Discord profile',
					id: 'enabled',
					default: true,
				})
				.addSwitch({
					label: 'Game Activity',
					description: "Show which game you're currently playing",
					id: 'activity',
					default: true,
					toggleable: {
						id: 'enabled',
						type: 'switch',
						value: true,
					},
				})
				.addSwitch({
					label: 'Play Time',
					description: "Display how long you've been playing",
					id: 'time',
					default: true,
					toggleable: {
						id: 'enabled',
						type: 'switch',
						value: true,
					},
				})
				.addSwitch({
					label: 'Join Button',
					description: 'Add a button for others to join your game session',
					id: 'joining',
					default: false,
					toggleable: {
						id: 'enabled',
						type: 'switch',
						value: true,
					},
				})
		)
		.build();
</script>

<style>
	/* Panel Container */
	.panel-container {
		animation: fadeIn 0.5s ease-in-out;
		background: linear-gradient(135deg, #f9f9f9, #e8e8e8);
		padding: 2rem;
		border-radius: 12px;
		box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
		transition: transform 0.3s, background-color 0.3s;
	}

	.panel-container:hover {
		background-color: #ffffff;
		transform: translateY(-5px);
	}

	/* Switch Styling */
	.switch-container {
		margin: 1rem 0;
		padding: 1rem;
		border: 1px solid #ddd;
		border-radius: 8px;
		background-color: #fafafa;
		transition: background-color 0.3s;
	}

	.switch-container:hover {
		background-color: #f1f1f1;
	}

	.switch-label {
		font-weight: bold;
		font-size: 1rem;
		color: #333;
	}

	.switch-description {
		color: #666;
		font-size: 0.9rem;
		margin-top: 0.2rem;
	}

	/* Fade-in Animation */
	@keyframes fadeIn {
		from {
			opacity: 0;
			transform: translateY(20px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	/* Responsive Design */
	@media (max-width: 768px) {
		.panel-container {
			padding: 1rem;
		}

		.switch-container {
			padding: 0.8rem;
		}

		.switch-label {
			font-size: 0.9rem;
		}

		.switch-description {
			font-size: 0.8rem;
		}
	}
</style>

<div class="panel-container">
	<Panel {panel} {render} on:switch={onSwitchClicked} />
</div>

