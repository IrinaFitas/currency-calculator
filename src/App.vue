<template>
	<div id="app">
		<h1>Currency calculator</h1>
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
		<p class="result"><strong>Result is: {{ result }} грн</strong></p>
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
		//This is by Vanilla JS

		// fetch("https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?json")
		// 	.then( res => res.json() )
		// 	.then ( data => {
		// 		data.forEach(elem => this.currenciesList.push(elem))
		// 	})
		// 	.catch( error => alert(`Упс! ${error}`));

		//This is by vue-resource
		this.$http.get("https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?json")
			.then( res => res.json() )
			.then ( data => {
				data.forEach(elem => this.currenciesList.push(elem))
			})
			.catch( error => alert(`Упс! ${error}`));
	}
}
</script>


<style lang="scss">
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	font-size: 16px;
	color: #2c3e50;
}
h1 {
	color: #293d3d;
}

.result {
	font-size: 20px;
}
form {
	display: flex;
	flex-direction: column;
	margin: auto;
	align-items: center;
}

.form-block {
	border-left: 2px solid #293d3d;
	border-right: 2px solid #293d3d;
	width: 50%;
	padding: 15px;
}
input, select {
	padding: 10px;
	border: 2px solid #293d3d;
}
button {
	background-color: #293d3d;
	color: white;
	font-size: 20px;
	border: none;
	padding: 10px 25px;
	border-radius: 5px;
	margin: 10px;
}
</style>

