<template>
    <div>
            Pregumtas
    </div>
</template>

<script>

import * as firebase from "firebase/app";
import "firebase/firestore";

    export default {
        data(){
            return{
                pregunta: '',
                opcion_a: '',
                opcion_b: '',
                opcion_c: '',
                opcion_d: '',
                respesta: '',
                pista: '',
                lista_preguntas: [],
                id_edita: '',               
                nivel: '',
                buscar: ''
            }    
        },
        methods : {
            show(){
                var db = firebase.firestore();
			   /*db.collection('preguntas_nivel1').onSnapshot(querySnapshot => {
				    //console.log(querySnapshot);
				    querySnapshot.forEach(prueb => {
					    console.log(prueb);					    
                    })
                    //console.log(querySnapshot);
                });*/
                db.collection('preguntas_nivel1').orderBy('numeral').get()
                .then((snapshot) => {
                    snapshot.forEach((doc) => {
                    console.log(doc.id, '=>', doc.data());
                    });
                })
                .catch((err) => {
                    console.log('Error getting documents', err);
                });
            }
        }
        ,
        mounted() {               
            //const admin = admin.initializeApp();
            this.show();
           // this.listar_usuarios();            
        }
    }
</script>

<style lang="scss" scoped>

</style>