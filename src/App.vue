<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col form-inline">
        <b-form-input v-model="newTask" placeholder="Enter Task" @keyup.enter="add"></b-form-input>
        <b-button class="ml-2" variant="primary" @click="add">Add</b-button>
      </div>
    </div>
    <div v-for="edge in allUsers.edges" :key="edge.node.id">{{edge.node.name}}</div>
    <input type="text"  v-model="newTask" @keyup.enter="onClicked" />
    <button @click="onClicked">Add User</button>

    <div class="row mt-3">
      <div class="col-md-3">
        <div class="p-2 alert alert-secondary">
          <h3>Backlog</h3>
          <draggable class="list-group kanban-column" :list="arrBacklog" group="task">
            <div class="list-group-item" v-for="element in arrBacklog" :key="element.name">
              {{element.name}}
            </div>
          </draggable>
        </div>
      </div>


      <div class="col-md-3">
        <div class="p-2 alert alert-primary">
          <h3>Progress</h3>
          <draggable class="list-group kanban-column" :list="arrInprogress" group="task">
            <div class="list-group-item" v-for="element in arrInprogress" :key="element.name">
              {{element.name}}
            </div>
          </draggable>
        </div>
      </div>



      <div class="col-md-3">
        <div class="p-2 alert alert-warning">
          <h3>Tested</h3>
          <draggable class="list-group kanban-column" :list="arrTested" group="task">
            <div class="list-group-item" v-for="element in arrTested" :key="element.name">
              {{element.name}}
            </div>
          </draggable>
        </div>
      </div>


      <div class="col-md-3">
        <div class="p-2 alert alert-success">
          <h3>Done</h3>
          <draggable class="list-group kanban-column" :list="arrDone" group="task">
            <div class="list-group-item" v-for="element in arrDone" :key="element.name">
              {{element.name}}
            </div>
          </draggable>
        </div>
      </div>



    </div>

  </div>
</template>

<script>

import draggable from "vuedraggable";
// import { useMutation } from '@vue/apollo-composable'
import gql from 'graphql-tag';


const createU = gql`
    mutation createUser($name:String){
     createUser(name: $name){
     ok
     }
    }


`

export default {
  name: 'App',
  apollo: {
    allUsers: gql`query {
            allUsers{
            edges{
              node{
                id
                name
              }
            }
           }
          }`,
  },
  components: {
    draggable
  },
  data(){
    return{
      newTask:"",
      arrBacklog:[
        {name:'Code refactoring1'},
        {name:'Code refactoring2'},
        {name:'Code refactoring3'},
        {name:'Code refactoring4'}
      ],
      arrInprogress:[],
      arrTested:[],
      arrDone:[],
    }
  },
  methods: {
    add() {
      if (this.newTask) {
        this.arrBacklog.push({name: this.newTask});
        this.newTask = "";
      }
    },

     onClicked() {
      console.log('Hi')
       this.$apollo.mutate({
        mutation: createU,
         variables:{
          name: this.name
         },


      }).then(() => {console.log('Done', this.name)})
    },
  }}
</script>

<style>
.kanban-column{
  min-height: 300px;
}
</style>
