<template>
    <div id="task">
        <!-- FORMULÁRIO PARA ADICIONAR TAREFAS -->
        <form @submit.prevent="addTask()">
            <input type="text" placeholder="Tarefa de hoje" v-model="task" />
            <button type="submit">Adicionar</button>
        </form>
        <!-- ITEMS -->
        <item :list="tasks" :delete="deleteTask"></item>
        <!-- TAREFAS PENDENTES -->
        <span v-if="tasks.length > 0">Você tem <b :class="{pendent: pendent}" >{{tasks.length}}</b> tarefa(s) pendentes</span>
    </div>
</template>

<script>

import Item from './Item';

export default {
    name: 'Task',
    components:{
        Item
    },
    data(){
        return{
            task: '',
            tasks: [],
            pendent: false
        }
    },
    methods:{
        addTask(){
            if(this.task !== ''){
                this.tasks.push({
                    text: this.task,
                    key: Date.now()
                });
            }else{
                alert('Informe uma tarefa');
                return;
            }
            // LIMPAR TAREFA
            this.task = '';
        },
        deleteTask(key){
            // PEGAR TODOS OS ITEMS DA LISTAGEM QUE SÃO DIFERENTES DA KEY
            let filterTasks = this.tasks.filter((item) => {
                return (item.key !== key);
            });
            // RETORNAR LISTAGEM ATUALIZADA
            return this.tasks = filterTasks;
        }
    },
    watch:{
        tasks:{
            deep: true,
            handler(){
                localStorage.setItem('tasks', JSON.stringify(this.tasks));
                this.tasks.length > 4 ? this.pendent = true : this.pendent = false;
            }
        }
    },
    // CHAMADO APÓS A INSTANCIA DO VUE SER CRIADA
    created(){
        // BUSCAR OS ITENS SALVOS NO STORAGE
        const json = localStorage.getItem('tasks');
        this.tasks = JSON.parse(json) || [];
    }
}
</script>

<style scoped>
    #task{
        max-width: 700px;
        background: #fff;
        border-radius: 4px;
        padding: 20px;
        margin: 20px auto;
        box-shadow: 0 0 20px rgba(0, 0, 0, .3);
    }
    form{
        margin-top: 30px;
        display: flex;
        flex-direction: row;
    }
    form button{
        cursor: pointer;
        background: #0f5959;
        border: 0;
        border-radius: 4px;
        margin-left: 10px;
        padding: 0 15px;
        display: flex;
        justify-content: center;
        align-items: center;
        color: #fff;
    }

    input{
        flex: 1;
        border: 1px solid #eee;
        padding: 6px 10px;
        border-radius: 4px;
        font-size: 14px;
        outline: none;
    }

    .pendent{
        color: red;
    }
</style>