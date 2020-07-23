<template>
<div>
    <div class="row">
 
        <!--<div class="py-5 col-md-6 row">
          <div class="col-1">
              <span class="fa fa-search fa-2x"></span>
          </div>
           <div class="col">
              <input class="form-control" @change="searchPerson" v-model="searchText"/>
          </div>
      </div>

-->


    <div class="col-md-6">

        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
          <div class="card">
            <div class="card-header">
              Mantenimiento Persona
            </div>
            <div class="card-body">
                <div class="d-none" id="txtId">
                    {{form.id}}
                </div>

                <b-form-group id="input-group-2" label="Nombre:" label-for="txtName">
                  <b-form-input
                    id="txtName"
                    v-model="form.name"
                    required
                    placeholder="Nombre completo"
                  ></b-form-input>
                  </b-form-group>

                  <b-form-group
                  id="txtDni"
                  label="D.N.I."
                  label-for="input-1"
                  description="Documento Nacional de Identidad"
                  >
                  <b-form-input
                    id="txtDni"
                    v-model="form.dni"
                    required
                    placeholder="D.N.I."
                    type="number" 
                  ></b-form-input>
                  </b-form-group>

                  <b-form-group
                  id="txtEmail"
                  label="E-mail:"
                  label-for="input-1"
                  >
                  <b-form-input
                  id="txtEmail"
                  v-model="form.email"
                  type="email"
                  required
                  placeholder="Ingresar e-mail"
                  ></b-form-input>
                    </b-form-group>
            </div>
            <div class="card-footer text-muted">
                <b-button type="submit" variant="primary" class="mr-md-4 mr-1 col-5">Guardar</b-button>
                <b-button type="reset" variant="danger" class="ml-md-4 ml-1 col-5">Reiniciar</b-button>
            </div>
          </div><!-- fin de card-->
          </b-form>
        </div>


    <div class="col-12 col-md-6" style="overflow-y: scroll; max-height: 60ex;">
        <table class="table table-hover table-striped table-borderless">
            <thead>
            <th>ID</th>
            <th>Nombre</th>
            <th>D.N.I.</th>
            <th>E-mail</th>
            <th></th>
            <th></th>
            </thead>
            <tbody>
                <tr v-for="(item, index) in this.list" :key="index++">

                    <td>
                        {{ item.id }}
                    </td>
                    <td>
                        {{ item.name }}
                    </td>
                    <td>
                        {{ item.dni }}
                    </td>
                    <td>
                        {{ item.email }}
                    </td>
                    <td>
                        <span class="fa fa-edit text-warning fa-2x personButton" v-on:click="personEdit(index)" data-target="tooltip" title="Editar"></span>
                    </td>
                    <td>
                        <span class="fa fa-close text-danger fa-2x personButton" v-on:click="personDelete(item.id, index)" data-target="tooltip" title="Eliminar"></span>
                       
                    </td>
                </tr>
            </tbody>
        </table>

    </div>

    </div>
        
  </div>
 
</template>

<script>
    import axios from 'axios'
  export default {
    mounted(){
        this.refreshList();
      },
    data() {
      return {
        form: {
          id: -1,
          name: 'Prueba',
          dni: 11111111,
          email: 'bla@bla.com'
        },
        list: null,
        show: true,
        searchText: ""
      }
    },
    methods: {
      personEdit(evt){
          //evt = el indice de la lista para obtener el object
          var obj = this.list[evt-1];
          this.form =  obj;
      },
      personDelete(id, index){
        //ejecuta POST
        axios.post("http://localhost:8080/unapersona/",      //uri
        JSON.stringify(this.form),   //data
        { headers: {
          'Content-type': 'application/x-www-form-urlencoded',
          }
        }).then(
            resp=>{
            console.log(resp);
            this.refreshList();
         });
        if(confirm("Confirma eliminar la persona " + id + "?"))
        {
            axios.post("http://localhost:8080/borrarpersona", //uri
            {"id": id},//data
            { headers: { //header por CROS
             'Content-type': 'application/x-www-form-urlencoded',
             }
            }).then(   //response
               resp=>{
               console.log(resp);
            });
            //remueve objeto de la lista
            this.list.splice(index-1,1);
        }
      },
      refreshList()
      {
        axios.get("http://localhost:8080/todaslaspersonas", 
        { headers: {
          'Content-type': 'application/x-www-form-urlencoded',
          }
        }).then(
            resp=>{
            this.list = resp.data;
         });
      },
      searchPerson()
      {
          alert(this.searchText);
      },
      onSubmit(evt) {
        evt.preventDefault()
        
        var uri;
        uri = (this.form.id <0) ? "http://localhost:8080/crearpersona" : "http://localhost:8080/actualizarpersona";
        //ejecuta POST
        axios.post(uri,      //uri
        JSON.stringify(this.form),   //data
        { headers: {
          'Content-type': 'application/x-www-form-urlencoded',
          }
        }).then(
            resp=>{
            console.log(resp);
            this.refreshList();
         });
      },
      onReset(evt) {
        evt.preventDefault()
        this.form.id = -1
        this.form.name = ''
        this.form.dni = ''
        this.form.email = ''
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    }
  }
</script>


*/