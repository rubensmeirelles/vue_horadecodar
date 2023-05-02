<template>
    <div>
        <Message :msg="msg" v-show="msg" />
        <div>
            <form id="burguer-form" @submit="createBurguer">
                <div class="input-container">
                    <label for="name">Nome do cliente</label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome">
                </div>
                <div class="input-container">
                    <label for="pao">Escolha um pão</label>
                    <select name="pao" id="pao" v-model="pao">
                        <option>Selecione o seu pão</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="carne">Escolha a carne</label>
                    <select name="carne" id="carne" v-model="carne">
                        <option>Selecione a sua carne</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
                    </select>
                </div>
                <div id="opcionais-container" class="input-container">
                    <label for="opcionais" id="opcionais-title">Escolha os opcionais</label>
                    <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
                        <input type="checkbox" name="opcionais" :value="opcional.tipo" v-model="opcionais">
                        <span>{{ opcional.tipo }}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" value="Criar meu Burguer" class="submit-btn">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import Message from './Message.vue';
    export default {
        name: "BurguerForm",
        data() {
            return {
                paes: null,
                carnes: null,
                opcionaisdata: null,
                nome: null,
                pao: null,
                carne: null,
                opcionais: [],
                status: "Solicitado",
                msg: null
            }
        },
        methods: {
            async getIngredientes() {
                const req = await fetch("http://localhost:3000/ingredientes");
                const data = await req.json();

                this.paes = data.paes;
                this.carnes = data.carnes;
                this.opcionaisdata = data.opcionais;
            },
            async createBurguer(e) {
                e.preventDefault();
                
                const data = {
                    nome: this.nome,
                    carne: this.carne,
                    pao: this.pao,
                    opcionais: Array.from(this.opcionais),
                    status: "Solicitado"
                }

                const dataJson = JSON.stringify(data);

                const req = await fetch("http://localhost:3000/burgers", {
                    method: "POST",
                    headers: {"Content-Type": "application/json"},
                    body: dataJson
                });

                const res = await req.json()

                this.msg = `Pedido nº ${res.id} realizado com sucesso!`

                setTimeout(() => this.msg = "", 3000);

                //limpar os campo
                this.nome = "";
                this.carne = "";
                this.pao = "";
                this.opcionais = "";

                console.log(res)
            }
        },
        
        mounted() {
            this.getIngredientes();
        },
        components:{
            Message
        }

}
</script>

<style scoped>
    #burguer-form{
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container{
        display: flex;
        flex-direction: column;
        justify-content: center;
        margin-bottom: 20px;        
    }

    label{
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid#fcba01;
    }

    input, select{
        padding: 5px 10px;
        width: 100%;
    }

    #opcionais-container{
        flex-direction: row;
        flex-wrap: wrap;
    }

    #opcionais-title{
        width: 100%;
    }

    .checkbox-container{
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
    }

    .checkbox-container span, .checkbox-container input{
        width: auto;
    }

    .checkbox-container span{
        margin-left: 6px;
        font-weight: bold;
    }

    .submit-btn{
        background-color: #222;
        color: #fcba01;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .submit-btn:hover{
        background-color: #fcba01;
        color: #222;
    }

</style>