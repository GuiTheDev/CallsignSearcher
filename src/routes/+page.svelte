<script lang=ts>
  
    let visible = false;
    let formData = {
        icao: '',
        iata: '',
        name: '',
    }
    let nvisible = false
    let iatacode = [String]
    let icaocode = [String]
    let airlinename = [String]
    let searchresults: Number
    let moredata: [String]
    function toggleVissible() {
        visible = true
    }

    function toggleNVis() {
        nvisible = true
    }

    const submitForm = () => {
        console.log("Your form data =>", formData)
        fetch("https://web-production-c91a.up.railway.app/getAirline?" + new URLSearchParams({
            iata: formData.iata,
            icao: formData.icao,
            airline: formData.name
        }))
        .then(response => response.json())
        .then(data => {
            console.log(data)
                moredata = data
                searchresults = data.length;
                airlinename = data[0][1]
                icaocode = data[0][2]
                iatacode = data[0][0]
            toggleVissible()
        }).catch(error => {
            console.log(error)
            return [];
        })

    }
    
</script>


<pagebody>
    <div class="all">
        <h2 style="text-align: center">Search your callsign!</h2>
        <br>
        <div class="forms">
            <div class="mb-3 icao">
                <label for="icaoform" class="form-label">ICAO code:</label>
                <input type="text" class="form-control" id="icaoform" bind:value={formData.icao}>
            </div>
            <div class="mb-3 iata">
                <label for="iataform" class="form-label">IATA code:</label>
                <input type="text" class="form-control" id="icaoform" bind:value={formData.iata}>
            </div>
            <div class="mb-3 name">
                <label for="nameform" class="form-label">Airline Name:</label>
                <input type="text" class="form-control" id="icaoform" bind:value={formData.name}>
                <div id="airlineName" class="form-text">
                   This must match exactly the airline name!
                  </div>
            </div>
            <form class="d-grid gap-2 search">
                <button on:click={submitForm} class="btn btn-primary" type="submit">Search!</button>
            </form>
        </div>
        <div class="aresults">
            {#if nvisible}
                <h3 style="text-align: center">No results found</h3>
            {/if}
        {#if visible}
            <h3 style="text-align: center">Found {searchresults} results</h3>
            <div class="results">
                <h3>Airline Name: {airlinename}</h3>
                <h3>ICAO: {icaocode}</h3>
                <h3>IATA: {iatacode}</h3>
            </div>
        {/if}

        <a href="/request"><p class="form-label cantfind " style="align-text: center">Can't find a callsign? Maybe we don't have it, you can request to add one here!</p></a>
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

    .search {
        margin-top: -5px;
    }
    
    .results {
        margin:auto;
        width: 50%;
        padding: 10px;
        padding-top: 5px;
    }
    .aresults {
        padding-top: 15px;
    }
    .cantfind {
        margin:auto;
        width: 50%;
        padding: 10px;
        padding-top: 5px;
    }
</style>