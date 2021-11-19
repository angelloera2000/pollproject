<script>
    
    let allData = [];
    let data = { name: "", idCard: "", email: "", phone: "" };

    if (localStorage.getItem("frm")) {
        allData = JSON.parse(localStorage.getItem("frm"));
    }

    let editStatus = false;

    const cleanProduct = () => {
        data = {
            name: "",
            idCard: "",
            email: "",
            phone: "",
        };
    };

    const saveData = () => {
        const newData = {
            name: data.name,
            idCard: data.idCard,
            email: data.email,
            phone: data.phone,
        };

        allData = allData.concat(newData);

        cleanProduct();
        console.log(allData);
        localStorage.setItem("frm", JSON.stringify(allData));
    };

    const editData = (dataEdited) => {
        editStatus = true;
        data = dataEdited;
        console.log(data);
    };

    //Se esta trabajando en esto por el momento
    const updateData = () => {
        let updateData = {
            name: data.name,
            idCard: data.idCard,
            email: data.email,
            phone: data.phone
        };
        const productIndex = allData.findIndex(p => p.id === data.id);
        allData[productIndex] = updateData;
        cleanProduct();
        editStatus = false;
    };

    const deleteData = (id) => {
        console.log(id);
        allData = allData.filter((data) => data.idCard !== id);
        localStorage.removeItem("frm");
    };

    const onSubmitHandler = () => {
            saveData();
    };
</script>

<main>
    <form on:submit|preventDefault={onSubmitHandler}>
        <h2 class="card-title">
            {#if !editStatus}Add Data{:else}Update Data{/if}
          </h2>
        <div style="float:left; width:200px;">
            <input placeholder="Name" bind:value={data.name} /> <br />
            <input placeholder="Id Card" bind:value={data.idCard} /> <br />
            <input placeholder="Email" bind:value={data.email} /> <br />
            <input placeholder="Phone" bind:value={data.phone} /> <br />
            <button type="submit">
                {#if !editStatus}Save Product{:else}Update Product{/if}
            </button>
        </div>
    </form>
    {#each allData as data}
        <table id="table">
            <tr>
                <td>{data.name}</td>
                <td>{data.idCard}</td>
                <td>{data.email}</td>
                <td>{data.phone}</td>
                <button on:click={deleteData(data.idCard)}> Delete </button>
                <!-- <button on:click={editData(data.idCard)}> Edit </button> -->
            </tr>
        </table>
    {/each}
</main>

<style>
</style>
