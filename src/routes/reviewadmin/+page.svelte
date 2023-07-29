<script lang=ts>
    let visible = false

    let formData = {
       email: '',
       password: ''
    }

    let id = ''
    let resp = [String]
    let youcanshow = false
    let login = false

    const submitForm = () => {
        fetch("https://web-production-c91a.up.railway.app/gettobeadded?" + new URLSearchParams({
                email: formData.email,
                password: formData.password,
            }))

            .then(response => response.json())
            .then(data => {
                console.log(data)
                

                if (data.sucess == false) {
                    console.log("Your are not an admin my friend")
                    login = false
                    return []
                }

                id = data
                console.log(id)
                resp = data
                visible = true
                login = true
                youcanshow = true
                
                
            }).catch(error => {
                console.log(error)
                return [];
            })
    }


    const alter = (index:string, approved:boolean) => {
        let operation = ''
        console.log(index)
        if (approved == true) {
            operation = 'add'
        }
        else if(approved == false) {
            operation = 'ignore'
        }

        fetch("https://web-production-c91a.up.railway.app/alter?" + new URLSearchParams({
                email: formData.email,
                password: formData.password,
                operation: operation,
                index: index

            }))

            .then(response => response.json())
            .then(data => {
                console.log(data)
                submitForm()
                login = true
                
                
                
            }).catch(error => {
                console.log(error)
                return [];
            })
                
    }
</script>


<pagebody>
    <div class="all">
        <h2 style="text-align: center">Admin Login</h2>
        <br>
        <div class="forms">
                <div class="mb-3 email">
                    <label for="emailform" class="form-label">Email: </label>
                    <input  name="email" type="email" class="form-control" id="emailform" bind:value={formData.email}>
                </div>
                <div class="mb-3 password">
                    <label for="passwordform" class="form-label">Password: </label>
                    <input name=password type="password" class="form-control" id="passwordform" bind:value={formData.password}>
                </div>
                <button class="btn btn-primary" type="submit" on:click={submitForm}>Login</button>  
        </div>
        <br>
        {#if youcanshow}

            {#if login}
                <div class="alert alert-success" role="alert">Successfully logged in</div>
            {/if}
            
            {#if !login}
                <div class="alert alert-danger" role="alert">Unable to Login, invalid credentials</div>
            {/if}
        {/if}
        {#if visible}
            <ul class="list-group">
                {#each resp as name, index}
                    <li class="list-group-item">{index} - {name}</li>
                    
                    <button type="button" class="btn btn-success sep" on:click={() => alter((name[3]).toString(), true)}>Approve</button>
                    <button type="button" class="btn btn-danger sep" on:click={() => alter((name[3]).toString(), false)}>Remove</button>
                
                {/each}
            </ul>
        {/if}
    </div> 
</pagebody>


<style>
    .forms {
        margin:auto;
        width: 50%;
        border: 3px solid gray;
        padding: 10px;
    }
    
    .all {
        margin-top: 40px;
    }

    .sep {
        margin-top: 10px;
    }
</style>