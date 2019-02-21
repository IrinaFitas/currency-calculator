<template>
	<div id="app">
		<h1>Current calculator</h1>
		<form action="" method="POST">
			<div class="form-block">
				<label><input type="radio" value="to_buy" v-model="operation">Buy</label>
				<label><input type="radio" value="to_sell" v-model="operation">Sell</label>
			</div>
			<div class="form-block">
				<label>Input the sum: <input type="text" v-model="sum"></label>
			</div>
			<div class="form-block">
				<select name="currency" v-model="selectedCurrency">
					<option v-for="(currency, index) in currenciesList" :value="currency.cc" :key="index"> {{ currency.txt }} </option>
				</select>
			</div>
			<button type="button" @click="calculate">Calculate</button>
		</form>
		<div>Result is: {{ result }} грн</div>
	</div>
</template>
<script>
export default {
	data() {
		return {
			currenciesList: [],
			selectedCurrency: "USD",
			operation: "to_buy",
			sum: 0,
			result: 0
		}
	},
	methods: {
		calculate() {
			if (this.operation === "to_buy") {
				this.currenciesList.forEach(elem => {
					if (this.selectedCurrency === elem.cc) {
						return this.result = (elem.rate + 0.2) * this.sum;
					}
				});
			} else {
				this.currenciesList.forEach(elem => {
					if (this.selectedCurrency === elem.cc) {
						return this.result = (elem.rate - 0.2) * this.sum;
					}
				});
			}
		}
	},
	created() {
		fetch("https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?json")
			.then( res => res.json() )
			.then ( data => {
				data.forEach(elem => this.currenciesList.push(elem))
			});
		console.log(this.currenciesList);
	}
}
</script>


<style lang="scss">
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
}
// #nav {
// 	padding: 30px;
// 	a {
// 		font-weight: bold;
// 		color: #2c3e50;
// 		&.router-link-exact-active {
// 		color: #42b983;
// 		}
// 	}
// }
form {
	display: flex;
	flex-direction: column;
	margin: auto;
	align-items: center;
}

.form-block {
	border: 1px solid black;
	width: 50%;
}
</style>

