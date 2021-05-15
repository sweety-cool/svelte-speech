<script>
	import { onMount } from "svelte";
	import { bind, text } from "svelte/internal";
	import {
		Col,
		Container,
		Row,
		Input,
		Label,
		FormGroup,
		Button,
	} from "sveltestrap";

	let voices = [];
	let rate = 1;
	let pitch = 1;
	let volume = 1;
	let TEXT = "";
	let selectedVoice;
	onMount(() => {
		speechSynthesis.onvoiceschanged = () => {
			voices = speechSynthesis.getVoices();
			selectedVoice = voices[0];
		};
	});

	function printVoice(voice) {
		if (!voice) {
			return "";
		}
		return `${voice.name} (${voice.lang})`;
	}
	function play() {
		const utterance = new SpeechSynthesisUtterance(TEXT);
		speechSynthesis.cancel();
		utterance.rate = rate;
		utterance.pitch = pitch;
		utterance.voice = selectedVoice;
		utterance.volume = volume;
		speechSynthesis.speak(utterance);
	}
</script>

<Container>
	<Row>
		<Col>
			<br />
			<br />
			<h1
				style="background:palevioletred; text-align: center;max-width: 700px;
			margin: auto;padding:1rem;"
			>
				SPEAK TO SWEETY
			</h1>
			<br />
			<br />
		</Col>
	</Row>
	<Row>
		<Col>
			<FormGroup>
				<Label for="words">Say something</Label>
				<input type="text" bind:value={TEXT} id="words" />
			</FormGroup>
		</Col>
	</Row>
	<Row>
		<Col>
			<FormGroup>
				<Label for="voices">Voices</Label>
				<Input type="select" bind:value={selectedVoice} id="voices">
					{#each voices as voice}
						<option value={voice}>{printVoice(voice)}</option>
					{/each}
				</Input>
			</FormGroup>
		</Col>
	</Row>
	<Row>
		<Col>
			<FormGroup>
				<Label for="pitch">Pitch</Label>
				<Input
					type="range"
					bind:value={pitch}
					min="0.1"
					max="2"
					step="0.1"
					id="pitch"
				/>
			</FormGroup>
		</Col>
	</Row>
	<Row>
		<Col>
			<FormGroup>
				<Label for="rate">Rate</Label>
				<Input
					type="range"
					bind:value={rate}
					min="0.1"
					max="2"
					step="0.1"
					id="rate"
				/>
			</FormGroup>
		</Col>
	</Row>
	<Row>
		<Col>
			<FormGroup>
				<Label for="volume">Volume</Label>
				<Input
					type="range"
					bind:value={volume}
					min="0.1"
					max="1"
					step="0.1"
					id="volume"
				/>
			</FormGroup>
		</Col>
	</Row>
	<Row>
		<Col>
			<FormGroup>
				<Button on:click={play} color="primary">Play</Button>
			</FormGroup>
		</Col>
	</Row>
	<Row>
		<Col>
			Pitch:{pitch}|Speed:{rate}|Volume:{volume}|voice:{printVoice(
				selectedVoice
			)}
		</Col>
	</Row>
</Container>
