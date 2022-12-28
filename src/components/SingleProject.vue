<template>
    <div class="project" :class="{complete: project.complete}">
        <div class="actions">
            <h3 @click="showDetails = !showDetails">{{project.title}}</h3>
            <div class="icons">
                <router-link :to="{name:'EditProject', params:{id:project.id}}"><span class="material-symbols-outlined">edit</span></router-link>
                <span @click="toggleComplete" class="material-symbols-outlined tick">done</span>
                <span @click="deleteProject" class="material-symbols-outlined delete">delete</span>
            </div>
        </div>
        <div v-if="showDetails" class="details"> 
            <p>{{project.details}}</p>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
export default {
    props:[
    "project"
    ],
    data(){
        return{
            showDetails:false,
            uri:'http://localhost:3000/projects/' + this.project.id
        }
    },
    methods:{
        async deleteProject(){
            try{
                const { data } = await axios.delete(this.uri);
                await this.$emit('delete', this.project.id)
            }catch(err){
                console.log(err)
            }
        },
        async toggleComplete(){
            try{
                const { data } = await axios.patch(this.uri, JSON.stringify({complete:!this.project.complete}));
                await this.$emit("complete", this.project.id)
            }catch(err){
                console.log(err)
            }
        }
    }
}
</script>

<style>
.project {
    margin: 20px auto;
    background: #2f4765;
    padding: 10px 20px;
    border-radius: 5px;
    box-shadow: 0 0px 5px 4px #ea4f30;
}
h3 {
    cursor: pointer;
}
.actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.material-symbols-outlined {
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
}
.material-symbols-outlined:hover {
    color: #777;
}
.delete:hover {
    color: #ea4f30;
}
.project.complete {
    box-shadow: 0 0px 5px 4px rgb(43,206,135);
}
.project.complete .tick {
    color: #35df90;
}
</style>