<template>
	<v-container>
		<v-row>
			<v-col cols="6">
				<v-card
					dense
				>
					<v-card-title>
						Digita la ciudad deseada
					</v-card-title>
					<v-card-text>
						<v-text-field
							label="Ciudad"
							v-model="city"
							@keydown.enter="getWeather()"
							hide-details
						>
						</v-text-field>
					</v-card-text>
					<v-card-actions>		
						<v-btn
							@click="getWeather()"
						>
							Buscar
						</v-btn>
					</v-card-actions>
				</v-card>
			</v-col>
			<v-col cols="6" v-if="showWeather">
				<v-card>
					<div class="d-flex justify-space-between">
						<div>
							<v-card-title>
								{{ this.datos.name }}
							</v-card-title>
							<v-card-subtitle>
								{{ this.datos.weather[0].description }}
							</v-card-subtitle>
						</div>
						<div>
							<v-avatar
								class="mx-4"
								size="100"
							>
								<img :src="imgUrl">
							</v-avatar>
						</div>
					</div>
				</v-card>
			</v-col>
			<v-col cols="6" v-if="error">
				<v-card>
					<div class="d-flex justify-space-between">
						<div>
							<v-card-title>
								Ciudad No Encontrada
							</v-card-title>
						</div>
						<div>
							<v-avatar
								class="mx-4"
								size="100"
							>
								<img src="not_found.png">
							</v-avatar>
						</div>
					</div>
				</v-card>
			</v-col>
			<v-col cols="6" v-if="loading">
				<v-card
					width="100px"
					height="100px"
					class="d-flex justify-center align-center"
					flat
				>
					<v-progress-circular
						indeterminate
						color="primary"
					></v-progress-circular>
				</v-card>
			</v-col>

		</v-row>
	</v-container>
</template>

<script>
import axios  from "axios";
export default {
	name: 'Clima',
	data() {
		return{
			city: null,
			showWeather: false,
			datos: [],
			imgUrl: "http://openweathermap.org/img/wn/",
			result: false,
			error: false,
			loading: false
		}
	},
	methods:{
		async getWeather() {
			this.showWeather = false;
			this.error = false;
			this.loading = true;
			this.imgUrl = "http://openweathermap.org/img/wn/";
			try {
				let datos = await axios.get('http://api.openweathermap.org/data/2.5/weather?q='+this.city+'&APPID=46e5721c63d023f333fad5b61fbf3eb6&lang=es')
				this.showWeather = true
				this.datos = datos.data
				this.imgUrl += this.datos.weather[0].icon + "@2x.png"
				console.log(this.datos)
				this.loading = false
				if(this.datos.cod == "200"){
					this.showWeather = true
				}
			} 
			catch (error) {
				console.log(error)
				this.loading = false
				this.error = true

			}
		}
	}
}
</script>

<style>

</style>