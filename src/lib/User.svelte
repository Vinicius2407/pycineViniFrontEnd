<script>
    let userSelection = 0;
    let resposta = "";
    let respostaDoGetByEmail = "";

    async function getUsers(){
        const res = await fetch('http://localhost:8000/users/list');
        const json = await res.json();
        resposta = json;
        let usuarios = []
        for (let i = 0; i < resposta.length; i++) {
            usuarios.concat(`<li>${resposta[i].name}</li>`)
        }
        return usuarios;
    }

    async function createUser(e){
        // envia o formulario no formato json
        let formData = new FormData(e.target);
        let data = Object.fromEntries(formData.entries());
        console.log(data);
        const res = await fetch('http://localhost:8000/user/create',{
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
        });
        const json = await res.json();
        resposta = JSON.stringify(json);
    }

    async function updateUser(e){
        // envia o formulario no formato json
        let formData = new FormData(e.target);
        let data = Object.fromEntries(formData.entries());
        const res = await fetch('http://localhost:8000/user/update',{
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
        });
        const json = await res.json();
        resposta = JSON.stringify(json);
    }

    async function deleteUser(){
        let email = document.getElementById("email").value;
        let password = document.getElementById("password").value;
        const res = await fetch("http://localhost:8000/users/search/?email=" + email + "&password=" + password);
        const json = await res.json();
        respostaDoGetByEmail = JSON.stringify(json);
        console.log(respostaDoGetByEmail);
        // let formData = new FormData(e.target);
        // let data = Object.fromEntries(formData.entries());
        // const res = await fetch('http://localhost:8000/user/delete',{
        //     method: 'DELETE',
        //     headers: {
        //         'Content-Type': 'application/json'
        //     },
        //     body: JSON.stringify(data)
        // });
        // const json = await res.json();
        // resposta = JSON.stringify(json);
    }
    </script>
    
    
    
    
    <div class="card">
        {#if userSelection === 0}
            <p>Selecione uma opção</p>
            <ul class="nav">
                <li><a href="/" on:click|preventDefault={() => (userSelection = 1)}>Ver Todos os Usuarios</a></li>
                <li><a href="/" on:click|preventDefault={() => (userSelection = 2)}>Criar Usuario</a></li>
                <li><a href="/" on:click|preventDefault={() => (userSelection = 3)}>Editar Usuarios</a></li>
                <li><a href="/" on:click|preventDefault={() => (userSelection = 4)}>Excluir Usuarios</a></li>
            </ul>
        {:else if userSelection === 1}
            <button on:click|preventDefault={getUsers}>Ver Usuarios</button>
            <ul>
                <li>Nome dos Usuarios</li>
                {#each resposta as user}
                    <li>Nome: {user.name}</li>
                    <li>Email: {user.email}</li>
                {/each}
            </ul>
            <button on:click|preventDefault={() => (userSelection = 0)}>Voltar</button>
        {:else if userSelection === 2}
            <form class="crud" on:submit|preventDefault={createUser}>
                <label for="name">Nome</label>
                <input type="text" name="name" id="name">
                <label for="email">Email</label>
                <input type="email" name="email" id="email">
                <label for="password">Senha</label>
                <input type="password" name="password" id="password">
                <input type="submit" value="Criar">
            </form>
            <button on:click|preventDefault={() => (userSelection = 0)}>Voltar</button>
        {:else if userSelection === 3}
            
            <button on:click|preventDefault={() => (userSelection = 0)}>Voltar</button>
        {:else if userSelection === 4}
            <form class="crud" on:submit|preventDefault={deleteUser}>
                <label for="email">Email</label>
                <input type="email" name="email" id="email">
                <label for="password">Senha</label>
                <input type="password" name="password" id="password">
                <input type="submit" value="Excluir">
            </form>
            <button on:click|preventDefault={() => (userSelection = 0)}>Voltar</button>
        {/if}
    </div>


        

    <h2>New user</h2>
    
    <!-- <p>{resposta}</p> -->
    
    
    
    <style>
    /* form.crud{
        display: grid;
        grid-template-columns: 1fr;
        gap: 5px;
        row-gap: 10px;
    }
    .crud input[type=submit]{
        justify-self: baseline;
    }
    .nav{
        display: flex;
        justify-content: space-around;
        list-style: none;
        padding: 0;

    } */
    </style>