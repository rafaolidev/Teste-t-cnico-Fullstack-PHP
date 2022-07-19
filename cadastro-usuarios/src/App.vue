<template>
		<div>
				<div class="table-wrapper">
						<table class="fl-table" id="tabela-alunos">
								<thead>
								<tr>
										<th>Nome</th>
										<th>Email</th>
										<th>Telefone</th>
										<th>Ações</th>
								</tr>
								</thead>
								<tbody v-for="user in users" :key="user.name">
										<tr>
												<td>{{user.name}}</td>
												<td>{{user.email}}</td>
												<td>{{user.phone}}</td>
												<td>
													<button v-on:click="deleteUserById(user.code)">deletar</button>
													<button>editar</button>
												</td>
										</tr>
								</tbody>
						</table>
				</div>
		</div>
		<div v-show="viewType == 0">
			<form @submit.prevent=""  class="form-data">
				<label for="name">
						Nome :
						<input type="text" id="name" placeholder="Nome" v-model="form.name">
				</label>
				<label for="email">
						E-mail:
						<input type="email" id="email" placeholder="email" v-model="form.email">
				</label>
				<label for="phone">
						Telefone:
						<input type="phone" id="phone"  placeholder="Telefone" v-model="form.phone">
				</label>
					<button v-on:click="
								() => {
									viewType = 1;
								}
							">PROXIMO</button>
			</form>
		</div>
		<div v-show="viewType == 1">
			<form @submit.prevent=""  class="form-data">
				<label for="professional">
						Experiência profissional:
						<input type="text" id="professionalExp" placeholder="experiência profissional" v-model="form.xpProf">
				</label>
				<label for="formacao">
						Formação acadêmica:
						<input type="text" id="formacao" placeholder="formação acadêmica" v-model="form.academic">
				</label>
					<button v-on:click="
								() => {
									viewType = 2;
								}
							">PROXIMO
					</button>
					<button v-on:click="
								() => {
									viewType = 0;
								}
							">ANTERIOR
					</button>
			</form>
		</div>
		<div v-show="viewType == 2">
			<form @submit.prevent=""  class="form-data">
				<label for="user">
						Usuário:
						<input type="text" id="userName" placeholder="Nome do Usuário" v-model="form.userName">
				</label>
				<label for="senha">
						Senha:
						<input type="text" id="senha" placeholder="senha" v-model="form.password">
				</label>
				<label for="confirmacaoSenha">
						Confirmação de senha:
						<input type="text" id="confirma"  placeholder="confirmação de senha" v-model="form.passwordConfirm">
				</label>
					<button v-on:click="
								() => {
									viewType = 1;
								}
							">ANTERIOR
					</button>
					<button v-on:click="submitForm(this.form)">ENVIAR
					</button>
			</form>
		</div>
</template>

<script>
import UserRepository from "./services/UserRepository.vue";

export default {
	name: 'App',
	data() {
		return {
			users:[],
			viewType :0,
			form: {
					name: '',
					email: '',
					xpProf: '',
					academic: '',
					userName: '',
					password: '',
					passwordConfirm:''
			}
		};
	},
	async created() {

		this.getUsersData();
		

	},
	methods:{

		async getUsersData() {
			this.users = [];
			const userApi = new UserRepository();
			let usersData =  (await userApi.getUsers()).data;
			this.users = usersData;
    },

		async deleteUserById(id) {

      const userApi = new UserRepository();

      userApi.deleteUser(id).then((resp) => {
        if (resp.status == 200) {
					this.getUsersData();
					console.log(resp.data.message);
        }
      });

    },

		submitForm(data){

			const userApi = new UserRepository();
			
			let passwordUnmatch = false;

			if (data.password != data.passwordConfirm) {
				passwordUnmatch =true;
			}

			if (passwordUnmatch == false) {

				userApi.createUser(data)
				this.getUsersData();
				
			}else{
				console.log('password diferente da confirmação')
			}

      

    }
		
	},
	components: {
	}
}
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}

.form-data{
width: 300px;
margin: auto;
display: flex;
flex-flow: column;
height: 400px;
justify-content: space-evenly;
}
form label {
display: flex;
flex-flow: column;
}

.table-wrapper{
	margin: 10px 70px 70px;
	box-shadow: 0px 35px 50px rgba( 0, 0, 0, 0.2 );
}

.fl-table {
	border-radius: 5px;
	font-size: 12px;
	font-weight: normal;
	border: none;
	border-collapse: collapse;
	width: 100%;
	max-width: 100%;
	white-space: nowrap;
	background-color: white;
}

.fl-table td, .fl-table th {
	text-align: center;
	padding: 8px;
}

.fl-table td {
	border-right: 1px solid #f8f8f8;
	font-size: 12px;
}

.fl-table thead th {
	color: #ffffff;
	background: rgba(0,0,0,0.7);
}


.fl-table thead th:nth-child(odd) {
	color: #ffffff;
	background: #eb8034;
}

.fl-table tr:nth-child(even) {
	background: #F8F8F8;
}

/* Responsive */

@media (max-width: 767px) {
	.fl-table {
		display: block;
		width: 100%;
	}
	.table-wrapper:before{
		content: "Scroll horizontally >";
		display: block;
		text-align: right;
		font-size: 11px;
		color: white;
		padding: 0 0 10px;
	}
	.fl-table thead, .fl-table tbody, .fl-table thead th {
		display: block;
	}
	.fl-table thead th:last-child{
		border-bottom: none;
	}
	.fl-table thead {
		float: left;
	}
	.fl-table tbody {
		width: auto;
		position: relative;
		overflow-x: auto;
	}
	.fl-table td, .fl-table th {
		padding: 20px .625em .625em .625em;
		height: 60px;
		vertical-align: middle;
		box-sizing: border-box;
		overflow-x: hidden;
		overflow-y: auto;
		width: 120px;
		font-size: 13px;
		text-overflow: ellipsis;
	}
	.fl-table thead th {
		text-align: left;
		border-bottom: 1px solid #f7f7f9;
	}
	.fl-table tbody tr {
		display: table-cell;
	}
	.fl-table tbody tr:nth-child(odd) {
		background: none;
	}
	.fl-table tr:nth-child(even) {
		background: transparent;
	}
	.fl-table tr td:nth-child(odd) {
		background: #F8F8F8;
		border-right: 1px solid #E6E4E4;
	}
	.fl-table tr td:nth-child(even) {
		border-right: 1px solid #E6E4E4;
	}
	.fl-table tbody td {
		display: block;
		text-align: center;
	}
}
</style>
