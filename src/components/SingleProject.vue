<template>
    <div class="project" :class="{complete:project.complete}">
        <div class="flexing">
            <div>
                <h3 @click="showDetail=!showDetail"> {{ project.title }} </h3>
            </div>

            <div>
                <span class="material-icons" @click="deleteProject">
                    delete
                </span>

                <router-link :to="{name:'EditProject',params:{id:project.id}}">
                    <span class="material-icons">
                        edit
                    </span>
                </router-link>
                
                <span class="material-icons" @click="completeProject">
                    check
                </span>
            </div>
        </div>

        <p v-if="showDetail"> {{ project.detail }} </p>
    
    </div>
</template>

<script>
export default {
    props:['project'],

    data(){
        return {
            showDetail: false,
            api: 'http://localhost:3000/projects/'
        }
    },

    methods:{
        deleteProject(){
            let deleteRoute = this.api+this.project.id;
            fetch(deleteRoute,{method:'DELETE'})    // method မထည့်ပေးရင် get request က auto အလုပ်လုပ်နေ
            .then(()=>{
                this.$emit("delete", this.project.id)
            })
            .catch((err)=>{
                console.log(err)
            })
        },

        completeProject(){
            let updateCompleteRoute = this.api+this.project.id
            fetch(updateCompleteRoute,
                {method:"PATCH",
                //object ထဲက သက်ဆိုင်ရာ Data တစ်ခုကိုပဲ ပြင်ချင်ရင် Patch ကိုသုံး
                headers:{
                    "Content-Type" : "application/json"
                },
                body:JSON.stringify(
                    {
                        complete: !this.project.complete
                    }
                )
            })
            .then(()=>{
                this.$emit("complete", this.project.id)
            })
            .catch((err)=>{
                console.log(err);
            })
        }
    }
}
</script>

<style>
    .project{
        padding:20px;
        background-color: #f2f2f2;
        margin: 10px;
        border-left: 6px solid crimson;
        border-radius: 8px;
    }

    h3{
        cursor: pointer;
        color: #00aeff;
    }

    .flexing{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    span{
        margin-left:10px;
        color:#2c3e50;
    }
    span:hover{
        cursor: pointer;
        color: #777;
    }

    .complete{
        border-left-color: green;
    }
</style>