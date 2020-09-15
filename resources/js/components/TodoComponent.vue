<template>
    <div class="w-50 ">
        <form @submit.prevent="savedata">
            <div class="form-row w-100 justify-content-center">
                <div class="form-group col-md-6">
                    <label for="inputEmail4">Add Todo</label>
                    <input type="text" v-model="title" class="form-control form-control-lg" placeholder="Add Todo">
                </div>
                <button type="submit" class="btn btn-success" style="height: border-box">ADD</button>
            </div>
        </form>

        <div class="w-25 justify-content-center pull-right" >
            <div v-for="todo in todos" :key="todo.id">
                {{todo.title}}
                <button class="btn btn-danger" @click="editdata(todo.id)">{{edit ? 'Save First ' : 'Edit'}}</button>
                <button class="btn btn-danger" @click="deletedata(todo.id)">Delete</button>
                <br>
            </div>

            <div>
                <div class="form-row justify-content-center">
                    <div class="form-group col-md-6">
                        <label v-if="edit" for="inputEmail4">Edit Todo</label>
                        <input type="text" v-model="edittitle" v-if="edit" class="form-control"
                               placeholder="Edit Todo ">
                        <button class="btn btn-danger" v-if="edit" @click="updatedata">Update</button>
                    </div>

                </div>
            </div>
        </div>
    </div>


</template>

<script>
    export default {
        data: function () {
            return {
                todos: '',
                title: '',
                err: '',
                edit: false,
                edittitle: '',
                editnumber: '',

            }
        },
        methods: {
            getTodos() {
                axios.get('http://127.0.0.1:8000/api/task').then((res) => {
                    this.todos = res.data;

                }).catch((error) => {
                    console.log(error);
                })
            },
            savedata() {


                axios.post('http://127.0.0.1:8000/api/task', {
                    data: {
                        title: this.title
                    }

                }).then((res) => {
                    this.getTodos();
                    this.$data.title = '';


                }).catch((error) => {
                    this.$data.err = error.response.data.title;
                });

            },
            editdata(a) {
                this.edit = true;
                this.editnumber = a;

                return a;

            },
            updatedata() {
                let a =this.editnumber;

                axios.put('http://127.0.0.1:8000/api/task/' + a, {
                    data: {
                        title: this.edittitle
                    }

                }).then((res) => {
                    this.getTodos();
                    this.$data.edittitle = '';
                    this.edit=false;


                })
            },
            deletedata(a) {

                axios.delete('http://127.0.0.1:8000/api/task/' + a, {}).then((res) => {
                    this.getTodos();
                    this.$data.title = '';


                })
            }

        },
        mounted() {

            this.getTodos();
        }

    }
</script>
