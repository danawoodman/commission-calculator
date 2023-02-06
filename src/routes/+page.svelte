<script lang="ts">
	let transactions = 10;
	let average_price = 600_000;
	let total_commission = 3;
	let commission_split = 30;
	let transaction_fee = 0;
	let brokerage_fee = 0;
	let has_cap = false;
	let cap = 0;

	$: total_sales = transactions * average_price;
	$: transaction_fees = transactions * transaction_fee;
	$: monthly_fees = brokerage_fee * 12;
	$: split = transactions * average_price * (total_commission / 100) * (commission_split / 100);
	$: final_split = has_cap && split >= cap ? cap : split;
	$: subtotal = transaction_fees + monthly_fees + split;
	$: commission_earnings = total_sales * (total_commission / 100);
</script>

<main>
	<section>
		<p>
			<label>
				Transaction:
				<input type="number" name="transactions" bind:value={transactions} />
			</label>
		</p>
		<p>
			<label>
				Average price: $<input type="number" name="average_price" bind:value={average_price} />
			</label>
		</p>
		<p>
			<label>
				Total commission:
				<input type="number" name="total_commission" bind:value={total_commission} />%
			</label>
		</p>
		<p>
			<label>
				Commission split:
				<input type="number" name="commission_split" bind:value={commission_split} />%
			</label>
		</p>
		<p>
			<label>
				Transaction fee: $<input
					type="number"
					name="transaction_fee"
					bind:value={transaction_fee}
				/>
			</label>
		</p>
		<p>
			<label>
				Brokerage fee: $<input type="number" name="brokerage_fee" bind:value={brokerage_fee} />
			</label>
		</p>
		<p>
			Has cap:
			<label><input type="radio" name="has_cap" bind:group={has_cap} value={true} /> Yes</label>
			<label><input type="radio" name="has_cap" bind:group={has_cap} value={false} /> No</label>
		</p>
		{#if has_cap}
			<p>
				$<input type="number" name="cap" bind:value={cap} />
			</p>
		{/if}
	</section>
	<section>
		<p>Total sales: {total_sales}</p>
		<p>Split total: {split}</p>
		<p>Transaction fees: {transaction_fees}</p>
		<p>Monthly fees: {monthly_fees}</p>
		<p>Subtotal: {subtotal}</p>
		<hr />
		<p>Commission earnings: {commission_earnings}</p>
		<p>Your earnings: {commission_earnings - final_split}</p>
		<p>Paid to broker: {final_split}</p>
	</section>
</main>

<style lang="postcss">
	main {
		display: flex;
		flex-direction: row;
		gap: 2rem;
		max-width: 60rem;
		margin: 5rem auto;
	}
	main > section {
		flex: 1;
	}
</style>
