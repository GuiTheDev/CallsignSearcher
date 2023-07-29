<script lang=ts>

    let visible = false

    let formData = {
        icao: '',
        iata: '',
        name: '',
    }
    function toggleVissible() {
        visible = true
    }


    const submitForm = () => {
        console.log("Your form data =>", formData)
        fetch("https://web-production-c91a.up.railway.app/requestAirline?" + new URLSearchParams({
            iata: formData.iata,
            icao: formData.icao,
            airline: formData.name
        }))
        .then(response => response.json())
        .then(data => {
            console.log(data)
            toggleVissible()
        }).catch(error => {
            console.log(error)
            return [];
        })
    }


</script>


<pagebody>
    <div class="all">
        <h2 style="text-align: center">Request a Callsign!</h2>
        <br>
        <div class="forms">
            <div class="mb-3 icao">
                <label for="icaoform" class="form-label">ICAO code:</label>
                <input required type="text" class="form-control" id="icaoform" bind:value={formData.icao}>
            </div>
            <div class="mb-3 iata">
                <label for="iataform" class="form-label">IATA code:</label>
                <input required type="text" class="form-control" id="icaoform" bind:value={formData.iata}>
            </div>
            <div class="mb-3 name">
                <label for="nameform" class="form-label">Airline Name:</label>
                <input required type="text" class="form-control" id="icaoform" bind:value={formData.name}>
                <div id="airlineName" class="form-text">
                   This must match exactly the airline name!
                  </div>
            </div>
            <form class="d-grid gap-2 search">
                <button on:click={submitForm} class="btn btn-primary" type="submit">Request!</button>
            </form>


            {#if visible}
            <h3 style="text-align: center" class="thanks">Request Sent! Thank you! ❤️</h3>
        {/if}
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

    .search {
        margin-top: -5px;
    }
    .thanks {
        margin-top: 25px;
    }
</style>