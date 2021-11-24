<script>
  import { v4 } from "uuid";
  import Noty from "noty";

  import "noty/lib/noty.css";

  let Questionnaires = [];

  let Questionnaire = {
    id: "",
    EF: "",
    MOA: "",
    LD: "",
    CAGEB: "",
    MZ: "",
    SG: "",
    ET: "",
    CL: "",
    NCV: "",
    TCV: "",
    CACCC: "",
    NE: "",
    NI: "",
    CFBUH: "",
  };

  if (localStorage.getItem("CENSO")) {
    Questionnaires = JSON.parse(localStorage.getItem("CENSO"));
  }

  $: localStorage.setItem("CENSO", JSON.stringify(Questionnaires));

  let editStatus = false;

  const cleanQuestionnaire = () => {
    Questionnaire = {
      id: "",
      ef: "",
      moa: "",
      ld: "",
      cageb: "",
      mz: "",
      sg: "",
      et: "",
      cl: "",
      ncv: "",
      tcv: "",
      caccc: "",
      ne: "",
      ni: "",
      cfbuh: "",
    };
  };

  const addQuestionnaire = () => {
    const newQuestionnaire = {
      id: v4(),
      ef: Questionnaire.ef,
      moa: Questionnaire.moa,
      ld: Questionnaire.ld,
      cageb: Questionnaire.cageb,
      mz: Questionnaire.mz,
      sg: Questionnaire.sg,
      et: Questionnaire.et,
      cl: Questionnaire.cl,
      ncv: Questionnaire.ncv,
      tcv: Questionnaire.tcv,
      caccc: Questionnaire.caccc,
      ne: Questionnaire.ne,
      ni: Questionnaire.ni,
      cfbuh: Questionnaire.cfbuh,
    };

    Questionnaires = Questionnaires.concat(newQuestionnaire);

    cleanQuestionnaire();
    localStorage.setItem("CENSO", JSON.stringify(Questionnaires));
  };

  const deleteQuestionnaire = (id) => {
    Questionnaires = Questionnaires.filter((item) => item.id !== id);
  };

  const editQuestionnaire = (QuestionnaireEdited) => {
    editStatus = true;
    Questionnaire = QuestionnaireEdited;
  };

  const updateQuestionnaire = () => {
    let updatedQuestionnaire = {
      id: Questionnaire.id,
      ef: Questionnaire.ef,
      moa: Questionnaire.moa,
      ld: Questionnaire.ld,
      cageb: Questionnaire.cageb,
      mz: Questionnaire.mz,
      sg: Questionnaire.sg,
      et: Questionnaire.et,
      cl: Questionnaire.cl,
      ncv: Questionnaire.ncv,
      tcv: Questionnaire.tcv,
      caccc: Questionnaire.caccc,
      ne: Questionnaire.ne,
      ni: Questionnaire.ni,
      cfbuh: Questionnaire.cfbuh,
    };
    const QuestionnaireIndex = Questionnaires.findIndex(
      (p) => p.id === Questionnaire.id
    );
    Questionnaires[QuestionnaireIndex] = updatedQuestionnaire;
    cleanQuestionnaire();
    new Noty({
      theme: "sunset",
      type: "success",
      timeout: 3000,
      text: "Se ha actualizado el cuestionario",
    }).show();
    editStatus = false;
  };

  const onSubmitHandler = () => {
    if (!editStatus) {
      addQuestionnaire();
    } else {
      updateQuestionnaire();
    }
  };
</script>

<main>
  <div class="container p-4">
    <div class="row">
      <div class="container p-8">
        <div class="row">
          <div class="col-md-6">
            {#each Questionnaires as Questionnaire}
              <div class="card mt-2 animate__animated animate__backInLeft">
                <div class="row">
                  <div class="col-md-8">
                    <div class="card-body">
                      <h5><strong>1. IDENTIFICACIÓN GEOGRÁFICA</strong></h5>
                      <h6>ENTIDAD FEDERATIVA</h6>
                      <p class="card-text">{Questionnaire.ef}</p>
                      <h6>MUNICIPIO O ALCALDÍA</h6>
                      <p class="card-text">{Questionnaire.moa}</p>
                      <h6>LOCALIDAD</h6>
                      <p class="card-text">{Questionnaire.ld}</p>
                      <h6>Clave de AGEB</h6>
                      <p class="card-text">{Questionnaire.cageb}</p>
                      <h6>MANZANA</h6>
                      <p class="card-text">{Questionnaire.mz}</p>
                      <h6>SEGMENTO</h6>
                      <p class="card-text">{Questionnaire.sg}</p>
                      <h5><strong>2. CONTROL DE VIVIENDA Y CUESTIONARIO</strong></h5>
                      <h6>ETIQUETA</h6>
                      <p class="card-text">{Questionnaire.et}</p>
                      <h6>CONSECUTIVO DE LAVIVIENDA</h6>
                      <p class="card-text">{Questionnaire.cl}</p>
                      <h6>NÚMERO DE CUESTIONARIO EN LA VIVIENDA</h6>
                      <p class="card-text">{Questionnaire.ncv}</p>
                      <h6>TOTAL DE CUESTIONARIOS EN LA VIVIENDA</h6>
                      <p class="card-text">{Questionnaire.tcv}</p>
                      <h5><strong>3. DIRECCIÓN DE LA VIVIENDA</strong></h5>
                      <h6>CALLE, AVENIDA, CALLEJON, CARRETERA, CAMINO</h6>
                      <p class="card-text">{Questionnaire.caccc}</p>
                      <h6>NÚMERO EXTERIOR</h6>
                      <p class="card-text">{Questionnaire.ne}</p>
                      <h6>NÚMERO INTERIOR</h6>
                      <p class="card-text">{Questionnaire.ni}</p>
                      <h6>
                        COLONIA, FRACCIONAMIENTO, BARRIO, UNIDAD HABITACIONAL
                      </h6>
                      <p class="card-text">{Questionnaire.cfbuh}</p>
                      <button
                        class="btn btn-danger"
                        on:click={deleteQuestionnaire(Questionnaire.id)}
                      >
                        Eliminar
                      </button>
                      <button
                        class="btn btn-secondary"
                        on:click={editQuestionnaire(Questionnaire)}
                      >
                        Editar
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            {/each}
          </div>
          <div class="col-md-6">
            <div class="card">
              <div class="card-body">
                <h2 class="card-title">
                  {#if !editStatus}Agregar cuestionario{:else}Actualizar
                    cuestionario{/if}
                </h2>
                <form on:submit|preventDefault={onSubmitHandler}>
                  <div class="form-group">
                    <p>1. IDENTIFICACIÓN GEOGRÁFICA</p>
                    <label for="product-name">ENTIDAD FEDERATIVA</label>
                    <input
                      bind:value={Questionnaire.ef}
                      type="text"
                      class="form-control"
                      id="Questionnaire-ef"
                      maxlength="2"
                      placeholder="Escriba la entidad federativa"
                    />
                  </div>

                  <div class="form-group">
                    <label for="product-description">MUNICIPIO O ALCALDÍA</label
                    >
                    <input
                      bind:value={Questionnaire.moa}
                      type="text"
                      class="form-control"
                      id="Questionnaire-moa"
                      maxlength="4"
                      placeholder="Escriba el municipio o alcadía"
                    />
                  </div>

                  <div class="form-group">
                    <label for="produtc-image-url">LOCALIDAD</label>
                    <input
                      bind:value={Questionnaire.ld}
                      type="text"
                      class="form-control"
                      id="Questionnaire-ld"
                      maxlength="4"
                      placeholder="Escriba la localidad"
                    />
                  </div>

                  <div class="form-group">
                    <label for="category">CLAVE AGEB</label>
                    <input
                      bind:value={Questionnaire.cageb}
                      type="text"
                      class="form-control"
                      id="Questionnaire-cageb"
                      maxlength="4"
                      placeholder="Escriba la clave AGEB"
                    />
                  </div>

                  <div class="form-group">
                    <label for="category">MANZANA</label>
                    <input
                      bind:value={Questionnaire.mz}
                      type="text"
                      class="form-control"
                      id="Questionnaire-mz"
                      maxlength="3"
                      placeholder="Escriba la manzana"
                    />
                  </div>

                  <div class="form-group">
                    <label for="category">SEGMENTO</label>
                    <input
                      bind:value={Questionnaire.sg}
                      type="text"
                      class="form-control"
                      id="Questionnaire-sg"
                      maxlength="1"
                      placeholder="Escriba el segmento"
                    />
                  </div>

                  <p>2. CONTROL DE VIVIENDA Y CUESTIONARIO</p>
                  <div class="form-group">
                    <label for="category">ETIQUETA</label>
                    <input
                      bind:value={Questionnaire.et}
                      type="text"
                      class="form-control"
                      id="Questionnaire-et"
                      maxlength="10"
                      placeholder="Escriba la etiqueta"
                    />
                  </div>

                  <div class="form-group">
                    <label for="category">CONSECUTIVO DE LAVIVIENDA</label>
                    <input
                      bind:value={Questionnaire.cl}
                      type="text"
                      class="form-control"
                      id="Questionnaire-cl"
                      maxlength="4"
                      placeholder="Escriba el consecutivo"
                    />
                  </div>

                  <div class="form-group">
                    <label for="category"
                      >NÚMERO DE CUESTIONARIO EN LA VIVIENDA</label
                    >
                    <input
                      bind:value={Questionnaire.ncv}
                      type="text"
                      class="form-control"
                      id="Questionnaire-ncv"
                      maxlength="2"
                      placeholder="Escriba el numero de cuestionario"
                    />
                  </div>

                  <div class="form-group">
                    <label for="category"
                      >TOTAL DE CUESTIONARIOS EN LA VIVIENDA</label
                    >
                    <input
                      bind:value={Questionnaire.tcv}
                      type="text"
                      class="form-control"
                      id="Questionnaire-tcv"
                      maxlength="2"
                      placeholder="Escriba el total de cuestionarios"
                    />
                  </div>

                  <p>3. DIRECCIÓN DE LA VIVIENDA</p>

                  <div class="form-group">
                    <label for="category"
                      >CALLE, AVENIDA, CALLEJON, CARRETERA, CAMINO</label
                    >
                    <input
                      bind:value={Questionnaire.caccc}
                      type="text"
                      class="form-control"
                      id="Questionnaire-caccc"
                      placeholder="Escriba la información"
                    />
                  </div>

                  <div class="form-group">
                    <label for="category">NÚMERO EXTERIOR</label>
                    <input
                      bind:value={Questionnaire.ne}
                      type="text"
                      class="form-control"
                      id="Questionnaire-ne"
                      placeholder="Escriba el número exterior"
                    />
                  </div>

                  <div class="form-group">
                    <label for="category">NÚMERO INTERIOR</label>
                    <input
                      bind:value={Questionnaire.ni}
                      type="text"
                      class="form-control"
                      id="Questionnaire-ni"
                      placeholder="Escriba el número interior"
                    />
                  </div>

                  <div class="form-group">
                    <label for="category"
                      >COLONIA, FRACCIONAMIENTO, BARRIO, UNIDAD HABITACIONAL</label
                    >
                    <input
                      bind:value={Questionnaire.cfbuh}
                      type="text"
                      class="form-control"
                      id="Questionnaire-cfbuh"
                      placeholder="Escriba la información"
                    />
                  </div>

                  <button type="submit" class="btn btn-primary">
                    {#if !editStatus}Guardar Cuestionario{:else}Actualizar
                      Cuestionario{/if}
                  </button>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</main>

<style>
</style>
