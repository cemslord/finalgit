<svelte:head>

<script type="module" src="https://unpkg.com/chessboard-element/bundled/chessboard-element.bundled.js"></script>
</svelte:head>
<script>
	import { onMount } from 'svelte';
  import { Chess } from 'chess.js';


let xxx ="bir"



	onMount( () => {

	fetch(`./aa1.pgn`).then((response)=>{
	return response.text()
	}).then(function(text) {
xxx= text
//console.log(xxx)
    });

const board = document.querySelector('chess-board');


	const game = new Chess();




		// update the board position after the piece snap
		// for castling, en passant, pawn promotion
		board.addEventListener('snap-end', (e) => {
		  board.setPosition(game.fen());
		});

	

	});

</script>

<main>


	<chess-board
    style="width: 400px"
    draggable-pieces
    drop-off-board="snapback"
    position="start">
</chess-board>

</main>

<style>

</style>
