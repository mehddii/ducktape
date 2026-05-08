<script lang="ts">
	import {
		Form,
		TextInput,
		Button,
		FormGroup,
		ButtonSet,
		FluidForm,
		PasswordInput,
		Link,
		ImageLoader
	} from 'carbon-components-svelte';
	import LogoGithub from 'carbon-icons-svelte/lib/LogoGithub.svelte';
	import UserFollow from 'carbon-icons-svelte/lib/UserFollow.svelte';
	import { LogoGitlab } from 'carbon-icons-svelte';
	import * as v from 'valibot';
	import logo from '$lib/assets/logo.png';

	let username = $state('');
	let email = $state('');
	let password = $state('');
	let confirmPassword = $state('');
	let error = $state('');

	const SignupSchema = v.object({
		username: v.pipe(v.string(), v.minLength(3)),
		email: v.pipe(v.string(), v.email()),
		password: v.pipe(v.string(), v.minLength(8)),
		confirmPassword: v.pipe(v.string(), v.minLength(8))
	});

	const singup = async (event: Event) => {
		event.preventDefault();

		const formValues = {
			username,
			email,
			password,
			confirmPassword
		};

		if (!v.is(SignupSchema, formValues)) {
			error = 'Invalid check the details and try again.';
		}
	};
</script>

<div class="h-screen overflow-hidden">
	<div class="flex min-h-screen items-center justify-center">
		<Form onsubmit={singup}>
			<FormGroup>
				<div class="grid grid-cols-2">
					<div class="self-end">
						<h1>Signup to DuckTape</h1>
						<p>And give your agents superpowers!</p>
					</div>
					<img
						class="col-star-1 h-35 w-auto justify-self-end object-contain"
						src={logo}
						alt="ducktape's logo"
					/>
				</div>
			</FormGroup>

			<FluidForm>
				<TextInput
					labelText="User name"
					placeholder="Enter user name..."
					invalidText="User name is already taken. Please try another."
					bind:value={username}
					required
				></TextInput>
				<TextInput labelText="Email" placeholder="Enter email..." bind:value={email} required
				></TextInput>
				<PasswordInput
					labelText="Password"
					placeholder="Enter password..."
					bind:value={password}
					required
				></PasswordInput>
				<PasswordInput
					labelText="Confirm password"
					placeholder="Confirm password..."
					bind:value={confirmPassword}
					required
				></PasswordInput>

				<FormGroup>
					<Button type="submit" class="min-w-full" icon={UserFollow}>Signup</Button>
				</FormGroup>
			</FluidForm>

			<hr class="my-6 h-0.5 border-t-0 bg-neutral-100" />

			<ButtonSet stacked class="gap-3">
				<Button icon={LogoGithub} class="min-w-full" kind="tertiary">Continue with GitHub</Button>
				<Button icon={LogoGitlab} class="min-w-full" kind="tertiary">Continue with GitLab</Button>
			</ButtonSet>

			<FormGroup class="my-4 flex justify-center">
				<p>Already Have an account? <span><Link href="/login">Sign in</Link> </span></p>
			</FormGroup>
		</Form>
	</div>

	<div class="items-bottom flex justify-center">
		<p>
			By proceeding, you agree to our <strong> Terms of Service </strong> and
			<strong> Privacy Policy </strong>.
		</p>
	</div>
</div>
