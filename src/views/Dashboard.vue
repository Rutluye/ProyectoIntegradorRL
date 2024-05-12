<template>
    <div>
        <Header/>

            <div class="container izquierda">

                <button class="btn btn-primary" v-on:click="nuevo()" >Nuevo cliente</button>
                <br><br>


                <table class="table table-hover">
                <thead>
                    <tr>
                        <th scope="col">ID</th>
                        <th scope="col">Nombre</th>
                        <th scope="col">DNI</th>
                        <th scope="col">TELEFONO</th>
                        <th scope="col">CORREO</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="paciente in Listapacientes" :key="paciente.PacienteId" v-on:click="editar(paciente.PacienteId)">
                        <th scope="row">{{ paciente.PacienteId}}</th>
                        <td>{{ paciente.Nombre }}</td>
                        <td>{{ paciente.DNI }}</td>
                        <td>{{ paciente.Telefono }}</td>
                        <td>{{ paciente.Correo }}</td>
                    </tr>
            
                </tbody>
                </table>

            </div>

        <Footer />
    </div>
</template>
<script>
import Header from '@/components/Header.vue';
import Footer from '@/components/Footer.vue';
import axios from 'axios';

window.watsonAssistantChatOptions = {
    integrationID: "b75552de-f4d5-4f06-998d-3d6eec533fff", // The ID of this integration.
    region: "au-syd", // The region your integration is hosted in.
    serviceInstanceID: "c0e43394-b5dc-47e7-a6d3-40953d7e19eb", // The ID of your service instance.
    onLoad: async (instance) => { await instance.render(); }
};

export default {
    name:"Dashboard",
    data(){
        return {
            Listapacientes:null,
            pagina:1
        }
    },
    components:{
        Header,
        Footer
    },
    methods:{
            editar(id){
                this.$router.push('/editar/' + id);
            },
            nuevo(){
                this.$router.push('/nuevo');
            }
    },
    mounted:function(){
        let direccion = "http://solodata.es/pacientes?page=" + this.pagina;
        axios.get(direccion).then( data =>{
            this.Listapacientes = data.data;
        });

        setTimeout(function(){
            const t=document.createElement('script');
            t.src="https://web-chat.global.assistant.watson.appdomain.cloud/versions/" + (window.watsonAssistantChatOptions.clientVersion || 'latest') + "/WatsonAssistantChatEntry.js";
            document.head.appendChild(t);
        });
    }
}
</script>

<style  scoped>
    .izquierda{
        text-align: left;
    }
</style>