<script>
	export let loginClasses = "modal";
	let data = [];

	let loginError = false;
	let user = { loggedIn: false };

	function noLoginError() {
		loginError = false;
	}

	function showLogin() {
		loginClasses = "modal is-active";
	}

	function hideLogin() {
		loginClasses = "modal";
	}

	function toggle() {
		user.loggedIn = !user.loggedIn;
	}

	function validateLoginForm(event) {
			let textbox = event.target;

			if(textbox.id == 'usuario'){
			}
			if(textbox.id == 'password'){
			}
			return true;
	 }

	async function handleSubmit(event) {
/*
			 console.log(event);
			 console.log(event.target);
			 console.log(event.target.usuario.value);
			 console.log(event.target.password.value);
*/
			 var form_data = new FormData();
       form_data.append('usuario', event.target.usuario.value);
       form_data.append('password', event.target.password.value);
       form_data.append('lang', 'es');

			const request = new Request('https://www.servidebate.com/api/v1.5.3/index.php/login', {
					method: 'POST',
					mode: 'cors',
					body: form_data
			 });
			 const response = await fetch(request);
			 const json = await response.json();
			 data = json;

			 if(data && data[0] && data[0].acceso){
				loginError = true;
			} else {
				hideLogin();
			}
	 }
</script>

<strong class="button is-info"
	on:click={showLogin}>Log in</strong>

<div id="modalLoginDiv" class="modal {loginClasses}">
  <div class="modal-background"></div>

	<form
			on:submit|preventDefault={handleSubmit}
			on:invalid={validateLoginForm}
			on:changed={validateLoginForm}
			on:input={validateLoginForm}
	>

  <div class="modal-card">
		{#if loginError}
		<div id="loginError" class="notification is-danger">
			<button class="delete" on:click={noLoginError}></button>
			No se pudo iniciar sesión. <strong>Usuario y Clave errados.</strong>
		</div>
		{/if}

    <header class="modal-card-head has-background-info">
			<span class="icon is-left">
				<i class="fas fa-user-shield fa-2x has-text-light"></i>
			</span>
      <p class="modal-card-title has-text-centered has-text-light">Login Screen</p>
      <button class="delete" on:click={hideLogin} aria-label="close"></button>
    </header>

    <section class="modal-card-body">
      <!-- Content ... -->

				<div class="field">
				  <div class="control has-icons-left has-icons-right">
				    <input required class="input" type="text"
						placeholder="User" id="usuario">
				    <span class="icon is-small is-left">
				      <i class="fas fa-user"></i>
				    </span>
						{#if !user.loggedIn}
				    <span class="icon is-small is-right">
				      <i class="fas fa-check"></i>
				    </span>
						{/if}
				  </div>
					{#if user.loggedIn}
				  	<p class="help is-danger">This user is invalid</p>
					{/if}

				</div>

				<div class="field">
				  <p class="control has-icons-left">
				    <input required class="input" type="password"
									 placeholder="Password" id="password">
				    <span class="icon is-small is-left">
				      <i class="fas fa-lock"></i>
				    </span>
				  </p>
				</div>

		</section>

    <footer class="modal-card-foot">
      <input type="submit" class="button is-success" value="Submit">
      <input class="button" on:click={hideLogin} value="Cancel">
    </footer>

  </div>
</form>

</div>
