<script>
	import Book from './book.svelte'
	import Button from './button.svelte'
	import Purchase from './purchase.svelte'

	let title = '';
	let price = 0;
	let description = '';

	let stock = [];
	let cart = [];

	function setTitle(event) {
		title = event.target.value;
	}

	function addBook() {
		const newBook = {
			title: title,
			price: price,
			description: description
		};
		stock = [...stock, newBook];
		title = '';
		price = 0;
		description = '';
	}

	function purchaseBook(title) {
		const purchasedBook = stock.find(book => book.title === title);
		cart = [...cart, purchasedBook];
	}

	function removeFromStock(title) {
		stock = stock.filter(book => book.title !== title);
		cart = cart.filter(book => book.title !== title);
	}
</script>

<style>
	h1 {
		color: purple;
		text-align: center;
	}

	section {
		margin: 1rem auto;
		width: 30rem;
	}

	label,
	input,
	textarea {
		width: 100%;
	}
</style>

<h1>Book Store</h1>

<section>
	<h2>Add New Book</h2>
	<div>
		<label for="title">Title</label>
		<input type="text" id="title" value={title} on:input={setTitle} />
	</div>

	<div>
		<label for="price"> Price</label>
		<input type="number" id="price" bind:value={price} />
	</div>

	<div>
		<label for="description">Description</label>
		<textarea rows="3" id="description" bind:value={description} />
	</div>

	<Button on:click={addBook}>ADD Book</Button>
</section>

<section>
	<h2>Stock</h2>
	{#if stock.length === 0}
		<p>No books in stock.</p>
	{:else}
		{#each stock as book}
			<Book
				bookTitle={book.title}
				bookPrice={book.price}
				bookDescription={book.description}
				on:buy={() => purchaseBook(book.title)}
				on:delete={() => removeFromStock(book.title)}
			/>
		{/each}
	{/if}
</section>

<section>
	<Purchase
		books={cart}
		onDelete={removeFromStock}
	/>
</section>
