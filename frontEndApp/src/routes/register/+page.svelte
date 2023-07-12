
<script>

    import axios from 'axios';
	import { redirect } from '@sveltejs/kit';

	let name = '', email = '', password = ''; 

	let submit = async () => {

		if (name == '' || email == '' || password == '') {
			return
		}

		console.log("hello")

		await axios.post('/register', {
			name,
			email,
			password
		})

		throw redirect(302, '/');

	}


</script>

<div id="login-page-container">

    <div id="login-box">

        <h2 style="color: aliceblue;">Register</h2>

        <div>
            <p style="margin-top: 2px;">User:</p>
            <input type="text" name="" id="" bind:value={name}>
            <p style="margin-top: 20px;">email:</p>
            <input type="text" name="" id="" style="margin-top: 2px;" bind:value={email}>
            <p style="margin-top: 20px;">Pass:</p>
            <input type="password" name="" id="" style="margin-top: 2px;" bind:value={password}>

            <div>
                <button style="margin-top: 75px;" on:click={submit}>Sing Up</button>
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
        height: 40px;

        border-radius: 7px;
    }

    #login-box div div {

        display: flex;
        flex-direction: column;
        flex-flow: column;

        align-items: center;
        justify-content: center;

        margin-top: 0px;

        width: 100%;
        height: 180px;

    }

    #login-box div div button {

        margin: 0px;
        margin-top: 50px;

        width: 100px;
        height: 30px;

        border-radius: 5px;


    }

</style>