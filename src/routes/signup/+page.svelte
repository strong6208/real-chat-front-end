<script lang="ts">
	import axios from "axios"
	import { onMount } from "svelte";
	import { goto } from "$app/navigation"
	import Fa from "svelte-fa"
	import { faWarning } from "@fortawesome/free-solid-svg-icons"

	export let errorMessage: string

	let formData = {
		firstName: '',
		lastName: '',
		email: '',
		password: '',
		confirmPassword: ''
  	}

  async function handleSubmit(): Promise<void> {
	if (formData.password === formData.confirmPassword) {
		try {
			const response = await axios.post("http://localhost:8080/api/register", formData)
	
			if (response.status === 200) {
				// Request was successful
				goto("/login")
			} else {
				// Request failed
				errorMessage = response.statusText
			}
		} catch (error) {
			errorMessage = "The Email already exists!"
		  console.error('Error:', error)
		}
	} else {
		errorMessage = "Please Confirm the Password!"
	}
  }

   /* Redirect if the user is logged in */
   	onMount(async () => {
        const token = localStorage.getItem("token")
        if (token) {
            goto("/chats")
        } 
	})
</script>

<section class="max-w-sm mx-auto mt-56">
	<div class="prose">
		<h1>Sign Up</h1>
	</div>
	<div class="flex gap-x-6 mt-4">
		<h1>Have Account ? </h1>
		<p>
			<a href="/login" class="link">Log In</a>
		</p>
	</div>

	<form
		class="flex flex-col gap-6 my-6"
		on:submit|preventDefault={handleSubmit}
	>
		{#if errorMessage}
			<div class="alert alert-error">
				<div>
					<Fa icon={faWarning} />
					{errorMessage}
				</div>
			</div>
		{/if}
		<p>
			<input
				type="firstName"
				name="firstName"
				placeholder="FirstName"
				class="input input-bordered w-full"
				required
				bind:value={formData.firstName}
			/>
		</p>
		<p>
			<input
				type="lastName"
				name="lastName"
				placeholder="LastName"
				class="input input-bordered w-full"
				required
				bind:value={formData.lastName}
			/>
		</p>
		<p>
			<input
				type="email"
				name="email"
				placeholder="Email..."
				class="input input-bordered w-full"
				required
				bind:value={formData.email}
			/>
		</p>
		<p>
			<input
				type="password"
				name="password"
				placeholder="Password..."
				class="input input-bordered w-full"
				required
				bind:value={formData.password}
			/>
		</p>
		<p>
			<input
				type="password"
				name="password-confirm"
				placeholder="Confirm password..."
				class="input input-bordered w-full"
				required
				bind:value={formData.confirmPassword}
			/>
		</p>
		<p class="flex items-center gap-6 mt-6">
			<button class="btn btn-primary w-full">Sign Up</button>
		</p>
	</form>
</section>
