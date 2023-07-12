<script>

	// --------------------------------------------------------------------------------------------------------------------------
	// Auth Axios Interceptors:

	// This simplify the other pages requests and axios functionality

	import { onMount, onDestroy } from 'svelte';
  	import axios from 'axios';

	axios.defaults.baseURL = 'http://127.0.0.1:8000';

	let isRefreshing = false;
	let failedQueue = [];
	let refreshTimeout;

	const processQueue = (error, token = null) => {
		failedQueue.forEach(prom => {
			if (error) {
			prom.reject(error);
			} else {
			prom.resolve(token);
			}
		});

		failedQueue = [];
	};

	const refreshToken = () => {
		if (isRefreshing) return;

		isRefreshing = true;

		axios.post('/refresh', {}, {withCredentials:true})
			.then(({data}) => {
			axios.defaults.headers.common['Authorization'] = `Bearer ${data.token}`;
			processQueue(null, data.token);
			// Set the timeout to refresh the token every 29 seconds
			refreshTimeout = setTimeout(refreshToken, 17000);
			})
			.catch(err => {
			processQueue(err, null);
			})
			.then(() => {
			isRefreshing = false;
			});
	};

	axios.interceptors.response.use(resp => resp, error => {
		const originalRequest = error.config;
		if (error.response.status === 401 && originalRequest.url === '/refresh') {
			return Promise.reject(error);
		}

		if (error.response.status === 401 && !originalRequest._retry) {
			if (isRefreshing) {
			return new Promise((resolve, reject) => {
				failedQueue.push({resolve, reject});
			}).then(token => {
				originalRequest.headers['Authorization'] = 'Bearer ' + token;
				return axios(originalRequest);
			}).catch(err => {
				return Promise.reject(err);
			});
			}

			originalRequest._retry = true;
			return refreshToken();
		}

		return Promise.reject(error);
	});

	onMount(refreshToken);

	onDestroy(() => {
		clearTimeout(refreshTimeout);
	});

</script>

<slot />
