<template>
	<div class="prod-signup-form">
		<b-card
			no-body
			style="max-width: 100%;"
			img-src="https://picsum.photos/600/300/?image=25"
			img-alt="Image"
			img-top
		>
			<b-card-body>
				<div role="group" class="form-group">
					<label for="input-name">Name</label>
					<b-form-input
						id="input-name"
						v-model="name"
						:state="nameState"
						aria-describedby="input-live-help input-live-feedback"
						placeholder="Enter your name"
						trim
					></b-form-input>
					<b-form-invalid-feedback id="input-live-feedback">
						This field is required.
					</b-form-invalid-feedback>
				</div>
				<div role="group" class="form-group">
					<label for="input-email">Email</label>
					<b-form-input
						id="input-email"
						v-model="email"
						:state="emailState"
						aria-describedby="input-live-help input-live-feedback"
						placeholder="Enter your email"
						type="email"
						trim
					></b-form-input>
					<b-form-invalid-feedback id="input-live-feedback">
						{{emailInvalidTxt}}
					</b-form-invalid-feedback>
				</div>
				<div role="group" class="form-group">
					<label for="input-product">Product</label>
					<b-form-input
						id="input-product"
						v-model="product"
						:state="productState"
						aria-describedby="input-live-help input-live-feedback"
						placeholder="Enter your product"
						trim
					></b-form-input>
					<b-form-invalid-feedback id="input-live-feedback">
						This field is required.
					</b-form-invalid-feedback>
				</div>
				<div role="group" class="form-group">
					<label for="input-country">Country</label>
					<b-form-select 
						id="input-country"
						v-model="country"
						:options="countries" 
						:state="countryState"
						aria-describedby="input-live-feedback"
						></b-form-select>
						<b-form-invalid-feedback id="input-live-feedback">
							This field is required.
						</b-form-invalid-feedback>

				</div>
				<div class="d-flex justify-content-end">
					<b-button variant="primary" @click="onSubmit">
						Submit
					</b-button>
				</div>
				<div :class="resClass" v-if="posted">
					<p>{{resultText}}</p>
				</div>
			</b-card-body>
		</b-card>
	</div>
</template>

<script>
import axios from 'axios';
export default {
	
	data(){
		return{
			name: '',
			nameState: null,
			email: '',
			emailState: null,
			emailInvalidTxt: "",
			product: '',
			productState: null,
			country: null,
			countryState: null,
			countries: [],
			posted: true,
			resultText: '',
			resClass: 'post-success'
		}
	},
	async created(){
		await this.initializeCounty();
	},
	methods:{
		async onSubmit(){
			const postData = {
				'name': "testName",
				'email': 'test@test1.com',
				'product': 'testProd',
				'country': 'testCountry'
			}
			const options = {
				headers: {'Access-Control-Allow-Origin': '*', 'Content-Type':'application/json'}
			};
			
			if (!this.checkValidity()){
				return;
			}
			axios.post('https://signups.danderdee.com/signup', postData)
				.then( response => {
					this.posted = true;
					this.postResult('post-success');
					this.resultText = "Your data was submitted successfully.";
				},
				error => {
					this.postResult('post-failed');
					this.resultText = "Sorry, Failed. Try again.";
				})
		},
		checkValidity(){
			this.nameState = this.name.length > 0 ? true : false;
			this.emailState = this.checkValidEmail();
			this.productState = this.product.length > 0 ? true : false;
			this.countryState = this.country != null ? true : false;
			return this.nameState && this.emailState && this.productState;
		},
		
		checkValidEmail(){
			if(this.email.length === 0){
				this.emailInvalidTxt = "This field is required";
				return false;
				
			}
			let re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
			if (!re.test(this.email.toLowerCase())){
				this.emailInvalidTxt = "This is invalid email";
				return false;
			}
			return true;
		},

		postResult(className){
			this.resClass = className;
		},

		async initializeCounty(){
			this.countries = [
				{ value: 'Russian Federation', text: 'Russian Federation'},
				{ value: "United Arab Emirates", text: 'United Arab Emirates'},
				{ value: "United Kingdom", text: 'United Kingdom'},
				{ value: 'United States', text:'United States'}
			];

			await axios.get('http://ip-api.com/json')
				.then(response =>{
					const curCountry = response.country;
					const idx =  this.countries.findIndex(item => item.value === curCountry )
					this.country = (idx === -1)? 'United Kingdom' : this.countries[idx];
				},
				error => {
					console.log("[country find error]", error);
					this.country = 'United Kingdom';
				});
		}

	}
}
</script>

<style scoped>
.post-success p{
	color: green;
}
.post-failed p{
	color: red;
}
</style>