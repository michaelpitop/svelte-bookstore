<script>
	export let books;
	export let onDelete;

	let initialValue = 0;

	$: total = books.reduce(function (accumulator, currentBook) {
		return accumulator + currentBook.price;
	}, initialValue);

	function removeBook(title) {
		onDelete(title);
	}
</script>

<h2>Cart</h2>
<ul>
	{#each books as book}
		<li>
			{book.title} - {book.price}
			<button on:click={() => removeBook(book.title)}>Remove</button>
		</li>
	{/each}
</ul>

<div class="price">Total Price: {total}</div>

<style>
	ul {
		list-style: none;
		margin: 1rem;
		padding: 0;
	}

	li {
		font-size: 1.5rem;
		margin: 1rem;
	}

	.price {
		border-top: 1px dashed black;
	}
</style>
