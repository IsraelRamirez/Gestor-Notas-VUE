<template>
	<div id="app">
		<h3>{{titulo}}</h3>
		<div class="form">
			<div class="form-group">
				<label >Título</label>
				<input type="text" class="form-control" v-model="nota.titulo">
			</div>
			<div class="form-group">
				<label>Texto</label>
				<textarea v-model="nota.texto" class="form-control"></textarea>
			</div>
			<button @click="agregarNota" class="btn btn-primary">Agregar Nota</button>
		</div>
		<div class="col-sm-12">
			<div class="col-sm-4 nota">
				<div v-for="nota in notas" class="card">
					<div class="card-block">
						<div class="card-title">
							{{nota.titulo}}
						</div>
						<div class="card-subtitle mb-2 text-muted">{{nota.fecha}}</div>
						<p class="card-text">{{nota.texto}}</p>
						<button @click="borrarNota(nota.id)" class="close"><b>&times</b> </button>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'app',
		data () {
			return {
				titulo: 'Gestión de notas',
				nota:{
					titulo: '',
					texto: ''
				},
				notas:[]
			}
		},
		methods:{
			agregarNota: function(){
				let {texto, titulo} = this.nota
				let id = 0
				if(this.notas.length != 0)
					id = this.notas[this.notas.length - 1].id + 1
				this.notas.push({
					id,
					titulo,
					texto,
					fecha: new Date(Date.now()).toTimeString()
				})
				this.guardarLocalStorage()
				this.nota.titulo = ''
				this.nota.texto = ''
			},
			guardarLocalStorage: function(){
				localStorage.setItem('notas', JSON.stringify(this.notas))
			},
			getLocalStorage: function(){
				let notas
				console.log(notas);
				if(localStorage.getItem('notas')){
					notas = JSON.parse(localStorage.getItem('notas'))
					this.notas = notas
				}
			},
			borrarNota: function(id){
				this.notas = this.notas.filter((nota) => nota.id != id)
				this.guardarLocalStorage()
			}
		},
		beforeMount(){
			this.getLocalStorage()
		}
	}
</script>

<style>
	.form{
		text-align: left;
	}

	.card{
		margin-top: 10px;
		margin-left: 5px;
		text-align: left;
		border: 1px solid #2c3e50;
		border-radius: 4px;
		padding-left: 8px;
		padding-right: 8px;
	}

	.nota{
		padding: 5px;
	}
</style>
