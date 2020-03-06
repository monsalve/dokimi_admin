<template>
    
    <div class="container-flex">
        <div class="card" >
            <div class="card-body">
                <h5 class="card-title">Usuarios</h5>
                <!--<h6 class="card-subtitle mb-2 text-muted">Card subtitle</h6>-->
                <p class="card-text">Configuraciones de usuarios</p>    

              
                <div class="form-group row">                
                    <div class="col-sm-4 col-md-4 col-4">
                        <input type="text" class="form-control "  v-model="buscar" name="buscar"  id="buscar">                              
                    </div>                  
                    <div class="col-sm-4 col-md-4 col-3">
                        <button type="button" class="btn btn-success" @click="listar_usuarios(1,buscar)" >Buscar</button>
                  
                        <button type="button"  class="btn btn-primary " id="btn_nuevo" data-toggle="modal" data-target="#exampleModalLong" @click="limpiar()">Nuevo</button>
                    </div>
                </div>
            </div>    
              <!-- TABLA LISTADO GENERAL -->
            <div  style="max-width: 95%;" class="table-responsive mr-2 ml-2 pl-2 pr-2">
                <table class="table table-bordered">
                    <thead class="thead-dark">
                        <th>#</th><th>Correo</th><th>Útimo acceso</th><th>-</th>
                    </thead>
                    <tbody>
                        <tr  v-for="(usuario ) in listado"  v-bind:key="usuario">
                            <td>{{ counter + 1}}</td>
                            <td v-text="usuario.email"></td>                        
                            <td v-if="usuario.status === '1'">Activo</td>
                            <td v-else>Inactivo</td>                                                    
                            <td>
                                <div style="min-width: 78px;">
                                    <template v-if="usuario.status">                              
                                        <!--<button type="button" @click="abrirModal('usuario','actualizar',usuario)" class="btn btn-warning btn-sm">
                                            
                                        </button> -->
                                        <button type="button" style="margin-right: 4px; !important"  class="btn btn-warning btn-sm " id="btn_actualiza" data-toggle="modal" data-target="#exampleModalLong" @click="editarUsuario(usuario.id)"><i class="far fa-edit"></i></button>
                                        &nbsp;                                  
                                        <button type="button" class="btn btn-danger btn-sm" @click="desactivarUsuario(usuario.id)">
                                            <i class="far fa-trash-alt"></i>
                                        </button>                                
                                    </template>
                                    <template v-else>
                                        <button type="button" title="Activar Usario" class="btn btn-secondary btn-sm" @click="activarUsuario(usuario.id)">
                                        <i class="far fa-check"></i>
                                        </button>
                                    </template>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table> 
                <nav>
                    <ul class="pagination">
                        <li class="page-item" v-if="pagination.current_page > 1">
                            <a class="page-link" href="#" @click.prevent="cambiarPagina(pagination.current_page - 1,buscar,criterio)">Ant</a>
                        </li>
                        <li class="page-item" v-for="page in pagesNumber" :key="page" :class="[page == isActived ? 'active' : '']">
                            <a class="page-link" href="#" @click.prevent="cambiarPagina(page,buscar,criterio)" v-text="page"></a>
                        </li>
                        <li class="page-item" v-if="pagination.current_page < pagination.last_page">
                            <a class="page-link" href="#" @click.prevent="cambiarPagina(parseInt(pagination.current_page) + 1,buscar,criterio)">Sig</a>
                        </li>
                    </ul>
                </nav>                
            </div>
        </div>
        <div class="modal fade" id="exampleModalLong" tabindex="-1" role="dialog" aria-labelledby="exampleModalLongTitle" aria-hidden="true">
     
        <div class="modal-dialog modal-xl" role="document">
            <div class="modal-content">

                <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLongTitle">Datos del usuario</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                </button>
                </div> 

                <form id="form_modal" class="needs-validation " novalidate @submit.prevent="enviado" >

                    <div class="modal-body">     

                        <div class="form-group row">
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
  <!-- MODAL PARA AGREGAR Y EDITAR USUARIOS-->  
</template>

<script>
//import * as firebase from "firebase/app";
//import "firebase/auth";
    export default {
        data(){
            return {               
               
                pagination : {
                    'total' : 0,
                    'current_page' : 0,
                    'per_page' : 0,
                    'last_page' : 0,
                    'from' : 0,
                    'to' : 0,
                },
                offset : 3,
                id_usu: '',
                id_usu_edita: '',                
                email: '',
                buscar : '',
                criterio: '',
                counter : 0,
                listado2: [],
                
                filtro_estado: '',                
                listado: [
                    {
                        'email' : 'ccccc',
                        'status': '1'
                    },
                    {
                        'email' : 'xxxxx',
                        'status': '2'
                    }
                    ,
                    {
                        'email' : 'wewew',
                        'status': '3'
                    }
                    ,
                    {
                        'email' : 'ghgfhghf',
                        'status': '4'
                    }
                ]
            }
                
        },
        computed:{
            isActived: function(){
                return this.pagination.current_page;
            },
            //Calcula los elementos de la paginación
            pagesNumber: function() {
                if(!this.pagination.to) {
                    return [];
                }
                
                var from = this.pagination.current_page - this.offset; 
                if(from < 1) {
                    from = 1;
                }

                var to = from + (this.offset * 2); 
                if(to >= this.pagination.last_page){
                    to = this.pagination.last_page;
                }  

                var pagesArray = [];
                while(from <= to) {
                    pagesArray.push(from);
                    from++;
                }
                return pagesArray;             

            }
        },
        methods : {
            listar_usuarios (page,buscar){
                console.log(page);
                console.log(buscar)
            },      

            editarUsuario(id_edita){
               
                console.log(id_edita);
            },

            actualizarUsuario(){
               
            },

            registrarUsuario(){
                
                
            },
            
            enviado(){
                
            },

            activarUsuario(id_uso_actvia){
                console.log(id_uso_actvia);
            },

            desactivarUsuario(id_uso_elim){
                console.log(id_uso_elim);
            },

            limpiar(){

            },           

            incrementCounter: function (index) {
                this.counter = ++this.counter          
                console.log(index);
            },

            cambiarPagina(page,buscar){
                let me = this;
                //Actualiza la páent_page = page;
                //Envia la petición para visualizar la data de esa página
                // console.log("page="+page);
                me.listar_usuarios(page,buscar);
            },
        },
        mounted() {            
            this.listar_usuarios(1,this.buscar);            
        }
    }
</script>

<style lang="scss" scoped>

</style>