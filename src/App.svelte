<script lang="ts">
	let movies: any[];
	const API_KEY: string = '489ce1a1';
	export let query: string;

	(async() => {
		const response = await fetch(`http://www.omdbapi.com/?apikey=${API_KEY}&s=${query}&plot=short`);
		let data = await response.json();
		movies = [...data.Search].reduce((container: any[], item) => {
			const objMovie = {
				id: item.imdbID,
				url: item.Poster.replace('X300', ''),
				title: item.Title
			};
			container.push(objMovie);
			return container;
		}, []);
		console.log(movies);
	})();
</script>

<main>
	<div class="loader-container">
		<div class="loader">
			<div></div>
			<div></div>
			<div></div>
			<div></div>
			<div></div>
			<div></div>
			<div></div>
			<div></div>
		</div>
	</div>
</main>

<style type="text/scss">
	:global(:root) {
		--primary: #3e2723;
		--light-primary: #d3b8ae;
		--dark-primary: #1b0000;
		--secondary: #bb4d00;
		--dark-secondary: #ac1900;
		--success: #558b2f;
		--error: #c62828;
		--white: #FFF;
		--color: #ffc107;
		--font-family: 'Orbitron', sans-serif;
	}

	main { height: 100%; }

	.loader-container {
		height: 100%;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		background: var(--dark-primary);
		filter: opacity(.9);
		color: var(--white);
	}

	.loader {
		display: inline-block;
		position: relative;
		width: 80px;
		height: 80px;

		div {
			animation: loader 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
			transform-origin: 40px 40px;

			$seconds: -0.036s;
			$top: 63px;
			$left: 63px;
			$topAux: 5px;

			&::after {
				content: " ";
				display: block;
				position: absolute;
				width: 7px;
				height: 7px;
				border-radius: 50%;
				background: var(--white);
				margin: -4px 0 0 -4px;	
			}

			@for $i from 1 through 8 {
				&:nth-child(#{$i}) {
					animation-delay: $seconds;
				}

				&:nth-child(#{$i})::after {
					top: $top;
					left: $left;
				}

				$seconds: $seconds + (-0.036s);
				$top: $top + $topAux;
				$left: $left - 7px;

				@if($i < 3) {
					$aux: ceil($topAux / 2);
					$topAux: $aux;
				} @else if($i == 3) {
					$topAux: $topAux * -1;
				} @else if($i > 3) {
					$aux: floor($topAux * 1.5);
					$topAux: $aux;
				}
			}
		}
	}

	@keyframes loader {
		0% {
			transform: rotate(0deg);
		}
		100% {
			transform: rotate(360deg);
		}
	}
</style>