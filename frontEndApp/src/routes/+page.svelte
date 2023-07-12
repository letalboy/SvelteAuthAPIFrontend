
<script>

	import axios from 'axios';
	import { redirect } from '@sveltejs/kit';

	let email = '', password = ''; 

	import { goto } from '$app/navigation';

	let submit = async () => {

		if (email == '' || password == '') {
			return
		}

		console.log("hello")

		const response = await axios.post('/login', {
			email,
			password
		}, {withCredentials: true});

		// We use "{withCredentials: true}" every time when we whant to get cookies from the
		// backend

		if (response.status == 200) { // Means that our response was succesfull
			
			axios.defaults.headers.common['Authorization'] = `Bearer ${response.data.token}`; // Store the cookies in the headers

			goto('/congrats');

		}
	}

</script>

<div id="login-page-container">

    <div id="login-box">

        <h2 style="color: aliceblue;">Login</h2>

        <div>
            <p style="margin-top: 2px;">User:</p>
            <input type="text" name="" id="" bind:value={email}>
            <p style="margin-top: 20px;">Pass:</p>
            <input type="password" name="" id="" bind:value={password} style="margin-top: 2px;">

            <div>
                <button on:click={submit} style="margin-top: 75px;">Sing In</button>
            </div>

            <div style="height: 10px; margin-top: 10px;">
                <a href="/register">create your acount here</a>
            </div>

        </div>
        
        
    </div>


</div>

<style>

    #login-page-container {
        display: flex;
        flex-direction: column;
        flex-flow: column;

        width: 100vw;
        height: 100vh;
        
        align-items: center;
        justify-content: center;

        background-color: #011936;
    }

    #login-box {
        display: flex;
        flex-direction: column;
        flex-flow: column;

        width: 500px;
        height: 600px;

        align-items: center;
        justify-content: center;
        
        border-radius: 22px;

        background-color: #50A2A7;

        /* box-shadow: 0px 0px 25px 6px rgba(0, 60, 100, 1); */
    }

    #login-box div {

        display: flex;
        flex-direction: column;
        flex-flow: column;

        margin-top: 70px;

        width: 70%;
        height: 400px;

    }

    #login-box div p {

        color: aliceblue;
        margin-left: 3px;

    }

    #login-box div input {
        
        width: 100%;
        height: 30px;

        border-radius: 7px;
    }

    #login-box div div {

        display: flex;
        flex-direction: column;
        flex-flow: column;

        align-items: center;
        justify-content: center;

        margin-top: 50px;

        width: 100%;
        height: 150px;

    }

    #login-box div div button {

        margin: 0px;
        margin-top: 50px;

        width: 100px;
        height: 30px;

        border-radius: 5px;


    }

</style>