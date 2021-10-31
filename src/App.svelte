<!-- Script -->
<script>
	import Header from "./Header.svelte"
	import Board from "./Board.svelte"

</script>

<!-- HTML -->
<html lang="en">
	<head></head>
	<body>
		<Header />
		<Board />
	</body>
</html>

<!-- Styles -->
<style>
	@import url('https://fonts.googleapis.com/css2?family=Zen+Kaku+Gothic+Antique:wght@300;400;500;700;900&display=swap');

	html {
		position: absolute;
		top: 0;
		left: 0;
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}
	
	body {
		width: 100%;
		height: 100%;
		background-color: rgb(24, 23, 28);
		color: white;
		font-family: 'Zen Koku Gothic Antique', sans-serif;
		font-size: 15px;
	}

	button {
		background: transparent;
		border: none;
		border-radius: 5px;
	}

</style>