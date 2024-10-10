<script>
    import {goto, stores} from '@sapper/app';
    import {post} from 'utils.js';

    const {session} = stores();
    let username = '';
    let name = '';
    let password = '';
    let error = null;
    let successMessage = null; // Для сообщения об успешной регистрации

    async function submit(event) {
        const response = await post(`auth/register`, {username, name, password});

        if (response.error) {
            error = response.error; // Если есть ошибка, выводим ее
            successMessage = null; // Скрываем сообщение об успехе
        } else if (response.id) {
            successMessage = "Registration successful!"; // Успешная регистрация
            error = null; // Скрываем сообщение об ошибке
            goto('/'); // Перенаправление на главную страницу
        }
    }
</script>

<svelte:head>
    <title>Sign up • BarBank</title>
</svelte:head>

<div class="auth-page">
    <div class="container page">
        <div class="row">
            <div class="col-md-6 offset-md-3 col-xs-12">
                <h1 class="text-xs-center">Sign up</h1>
                <p class="text-xs-center">
                    <a href="/login">Have an account?</a>
                </p>
                {#if error}
                    <div class="alert alert-danger" role="alert">{error}</div>
                {/if}
                {#if successMessage}
                    <div class="alert alert-success" role="alert">{successMessage}</div>
                {/if}

                <form on:submit|preventDefault={submit}>
                    <fieldset class="form-group">
                        <input class="form-control form-control-lg" type="text" required placeholder="Your username"
                               bind:value={username}>
                    </fieldset>
                    <fieldset class="form-group">
                        <input class="form-control form-control-lg" type="text" required placeholder="Name"
                               bind:value={name}>
                    </fieldset>
                    <fieldset class="form-group">
                        <input class="form-control form-control-lg" type="password" required placeholder="Password"
                               bind:value={password}>
                        {#if password.length > 1 && password.length < 6 }
                            <sup>
                                <div class="alert alert-danger" role="alert">Password too short</div>
                            </sup>
                        {/if}
                    </fieldset>
                    <button class="btn-signup">
                        Sign up
                    </button>
                    <button class="btn-secondary">
                        Cancel
                    </button>
                </form>
            </div>
        </div>
    </div>
</div>

<style>
    .form-group {
        margin-bottom: 1rem; /* Отступ между полями ввода */
    }

    .btn-signup, .btn-secondary {
        font-size: 1.2rem;
        padding: 0.75rem 1.5rem;
        color: white;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        transition: background-color 0.3s ease;
        margin-bottom: 1rem; /* Отступ между кнопками */
        display: block; /* Устанавливает кнопку как блочный элемент для выравнивания */
        width: 100%; /* Ширина кнопок 100% для выравнивания */
    }

    .btn-signup {
        background-color: blue;
    }

    .btn-signup:hover {
        background-color: #0056b3;
    }

    .btn-secondary {
        background-color: blue; /* Цвет для вторичной кнопки */
    }

    .btn-secondary:hover {
        background-color: #5a6268; /* Более темный цвет при наведении */
    }
</style>
