<template>
    <div>
        <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
            <a class="navbar-brand" href="#" @click="rutear('/')">Dokimi</a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>

            <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav mr-auto">                
                <li class="nav-item">
                    <a class="nav-link" @click="rutear('usuarios')" href="#">Usuarios</a>
                    
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                    Dropdown
                    </a>
                    <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                    <a class="dropdown-item" href="#">Action</a>
                    <a class="dropdown-item" href="#">Another action</a>
                    <div class="dropdown-divider"></div>
                    <a class="dropdown-item" href="#">Something else here</a>
                    </div>
                </li>
                <li class="nav-item">
                    <a class="nav-link disabled" href="#">Disabled</a>
                </li>
                </ul>
                <form class="form-inline my-2 my-lg-0">
                
                <button class="btn btn-danger" @click="signOut">Salir</button>
                </form>
            </div>
            </nav>
        <div>
            
        </div>
    </div>
</template>

<script>
import * as firebase from "firebase/app";
import "firebase/auth";
 
    export default {
        created(){
            firebase.auth().onAuthStateChanged( user =>{
                this.loggedIn = !!user;       
                console.log(user)         ;
            });
        },
        data(){
            return {
                loggedIn: false
            }
        },
        methods: {

            rutear(ruta_ir){
                //console.log("sdfsfsdfsd");
                this.$router.replace({name: ruta_ir});
            },
            async signOut(){
                try {
                    const data = await firebase.auth().signOut();
                    console.log(data);
                    this.$router.replace({name: "login"});            
                }
                catch(err){
                    console.log(err);
                }
                
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>