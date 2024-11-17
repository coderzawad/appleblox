<script lang="ts">
	import { os } from '@neutralinojs/lib';
	import { Github, MessageSquare } from 'lucide-svelte';
	import { SettingsPanelBuilder } from '../components/settings';
	import Panel from '../components/settings/panel.svelte';
	import Informations from '../components/informations/informations.svelte';

	export let render = true;

	function onButtonClicked(e: CustomEvent) {
		const { id } = e.detail;
		switch (id) {
			case 'discord_btn':
				os.open('https://appleblox.com/discord');
				break;
			case 'github_btn':
				os.open('https://github.com/AppleBlox/appleblox');
				break;
		}
	}

	const panel = new SettingsPanelBuilder()
		.setName('About')
		.setId('info')
		.setDescription('About AppleBlox and helpful resources')
		.addCategory((category) =>
			category.setId('contributors').addCustom({
				label: '',
				description: '',
				id: 'info_page',
				component: Informations,
			})
		)
		.addCategory((category) =>
			category
				.setName('Links')
				.setDescription('Community and development resources')
				.setId('resources')
				.addButton({
					label: 'Discord Server',
					description: 'Get help and connect with the community',
					id: 'discord_btn',
					variant: 'outline',
					icon: { component: MessageSquare },
				})
				.addButton({
					label: 'Github',
					description: 'View source code, report issues and contribute',
					id: 'github_btn',
					variant: 'outline',
					icon: { component: Github },
				})
		)
		.build();
</script>

<style>
	/* Page Animations */
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

	@keyframes hoverPop {
		0% {
			transform: scale(1);
		}
		50% {
			transform: scale(1.05);
		}
		100% {
			transform: scale(1);
		}
	}

	.panel-container {
		animation: fadeIn 0.5s ease-in-out;
		padding: 2rem;
		background: linear-gradient(135deg, #ece9e6, #ffffff);
		box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
		border-radius: 12px;
	}

	.button {
		transition: transform 0.3s, background-color 0.3s;
		cursor: pointer;
		background-color: #f4f4f4;
		border: 2px solid #ddd;
		padding: 1rem;
		border-radius: 8px;
		display: flex;
		align-items: center;
		gap: 1rem;
	}

	.button:hover {
		background-color: #eef2f5;
		transform: translateY(-5px);
		animation: hoverPop 0.5s ease-in-out;
	}

	.button-icon {
		width: 24px;
		height: 24px;
	}

	.button-label {
		font-weight: bold;
		font-size: 1.1rem;
		color: #333;
	}

	.description {
		color: #666;
		font-size: 0.9rem;
	}

	@media (max-width: 768px) {
		.panel-container {
			padding: 1.5rem;
		}

		.button {
			padding: 0.8rem;
			gap: 0.5rem;
		}

		.button-label {
			font-size: 1rem;
		}
	}
</style>

<div class="panel-container">
	<Panel {panel} on:button={onButtonClicked} {render} autosave={false} />
</div>
