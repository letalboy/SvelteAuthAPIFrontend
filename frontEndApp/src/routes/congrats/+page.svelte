<script>

    import axios from 'axios';
	import { redirect } from '@sveltejs/kit';

    let user;

    import {onMount, onDestroy} from "svelte";

    let date_joined;
    let email;
    let groups;
    let id;
    let is_active;
    let is_staff;
    let is_superuser;
    let last_login;
    let name;
    let user_permissions;

    function formatDateTime(date) {

        if (date != null || date != undefined) {

        } else {
            return ""
        }

        const day = String(date.getDate()).padStart(2, '0');
        const month = String(date.getMonth() + 1).padStart(2, '0'); // January is 0!
        const year = date.getFullYear();

        const hours = String(date.getHours()).padStart(2, '0');
        const minutes = String(date.getMinutes()).padStart(2, '0');
        const seconds = String(date.getSeconds()).padStart(2, '0');

        return `${day}/${month}/${year} - ${hours}:${minutes}:${seconds}`;
    }

    let get_user = async () => { 
                
        try {

            const response = await axios.get('/get_user', {withCredentials: true});

            user = (response.data)

            date_joined = formatDateTime(new Date(user.date_joined));
            email = user.email;
            groups = user.groups;
            id = user.id;
            is_active = user.is_active;
            is_staff = user.is_staff;
            is_superuser = user.is_superuser;
            last_login = formatDateTime(new Date(user.last_login));
            name = user.name;
            user_permissions = user.user_permissions;

        } catch (error) {
            if (error.response && error.response.status === 401) {

            } else {
                console.log("An unexpected error occurred: ", error.message);
            }
        }

    };   

    $: console.log(user);

    onMount(() => {
        get_user();
    });

    const intervalId = setInterval(get_user, 3000);

    onDestroy(() => {
    clearInterval(intervalId);
    });

</script>


<div id="congrats-page-container">

    <h1 style="color: aqua; margin-top: -55px;">Application is working!</h1>

    <div>
        <p>date joined:  {date_joined}<br>
        email: {email}<br>
        groups: {groups}<br>
        id:  {id}<br>
        is_active:  {is_active}<br>
        is_staff:  {is_staff}<br>
        is_superuser:  {is_superuser}<br>
        last_login:  {last_login}<br>
        name:  {name}<br>
        user_permissions: {user_permissions}</p>
    </div>

</div>

<style>

    #congrats-page-container {
        display: flex;
        flex-direction: column;
        flex-flow: column;

        width: 100vw;
        height: 100vh;
        
        align-items: center;
        justify-content: center;

        background-color: #011936;
    }

    #congrats-page-container div {
        display: flex;
        flex-direction: column;
        flex-flow: column;

        align-items: center;
        justify-content: center;

        margin-top: 50px;

        width: 300px;
        height: 100px;

    }

    #congrats-page-container p {
        color: aliceblue;
    }


</style>