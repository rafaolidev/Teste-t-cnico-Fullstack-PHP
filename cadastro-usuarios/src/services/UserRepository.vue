<script>
import axios from 'axios';

export default class UserRepository {

	constructor() {
		this.axiosService = axios.create({
			baseURL: "http://localhost/api_teste_selecty.php",
		});
	}

    async getUsers(){
     return this.axiosService.get()
    }

    async deleteUser(userId){
    return this.axiosService.delete(`?user_code=${userId}`)
    }

    async createUser(user){
    axios.post('http://localhost/api_teste_selecty.php', {
    jsonData: {
        name:user.name,
        email:user.email,
        user_name:user.userName,
        phone:user.phone,
        password:user.password,
        xp:user.xpProf,
        academic: user.academic
    }
    })
    .then(function (response) {
    console.log(response);
    })
    .catch(function (error) {
    console.log(error);
});
     
    }


}
</script>