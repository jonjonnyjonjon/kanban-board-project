<template>
    <div class="container mt-5">
        <div class="row">
            <h1 class="ml-3" @dblclick="editBoardTitle = true" v-if="editBoardTitle === false">{{boardTitle}}</h1>
            <input class="ml-3" v-if="editBoardTitle === true" v-model="boardTitle" @keyup.enter="updateBoardTitle()">
        </div>

        <div class="row ml-1">
            <p @dblclick="editBoardDescription = true" v-if="editBoardDescription === false">{{boardDescription}}</p>
            <textarea rows="4" cols="50" v-if="editBoardDescription === true" v-model="boardDescription" @keyup.enter="updateBoardDescription()"></textarea>
        </div>

        <div class="row">
            <div class="col form-inline">
                <b-form-input v-model="newTask" placeholder="Enter Task" @keyup.enter="add"></b-form-input> <b-button class="ml-2" variant="primary" @click="add">Add</b-button>
            </div>
        </div>

        <div class="row mt-3">
            <div class="col-md-4">
                <div class="p2 alert alert-danger">
                    <h3>To Do</h3>
                    <draggable class="list-group kanban-column arrayToDo" :list="arrayToDo" group="tasks" :move="checkMove">
                        <div class="list-group-item" v-for="element in arrayToDo" :key="element.name">
                            {{element.name}}
                            <div class="float-right">
                                <b-button size="sm" variant="danger" @click="onDelete('arrayToDo', element)">
                                    <trash-2-icon class="custom-class"></trash-2-icon>
                                </b-button>
                            </div>
                        </div>
                        <!-- input box inside column, add directly to column -->
                        <!-- <button slot="header" @click="addPeople">Add</button> -->
                    </draggable>
                </div>
            </div>

            <div class="col-md-4">
                <div class="p2 alert alert-primary">
                    <h3>In Progress</h3>
                    <draggable class="list-group kanban-column arrayInProgress" :list="arrayInProgress" group="tasks" :move="checkMove">
                        <div class="list-group-item" v-for="element in arrayInProgress" :key="element.name">
                            {{element.name}}
                            <div class="float-right">
                                <b-button size="sm" variant="danger" @click="onDelete('arrayInProgress', element)">
                                    <trash-2-icon class="custom-class"></trash-2-icon>
                                </b-button>
                            </div>
                        </div>
                    </draggable>
                </div>
            </div>

            <div class="col-md-4">
                <div class="p2 alert alert-success">
                    <h3>Done</h3>
                    <draggable class="list-group kanban-column arrayDone" :list="arrayDone" group="tasks" :move="checkMove">
                        <div class="list-group-item" v-for="element in arrayDone" :key="element.name">
                            {{element.name}}
                            <div class="float-right">
                                <b-button size="sm" variant="danger" @click="onDelete('arrayDone', element)">
                                    <trash-2-icon class="custom-class"></trash-2-icon>
                                </b-button>
                            </div>
                        </div>
                    </draggable>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import draggable from "vuedraggable";
import { Trash2Icon } from "vue-feather-icons";

export default {
    name: "App",
    components: {
        draggable,
        Trash2Icon 
    },
    data() {
        return {
            boardTitle: "Kanban Board",
            editBoardTitle: false,
            boardDescription: "This is a Kanban Board. (double click to edit)",
            editBoardDescription: false,
            newTask: "",
            arrayToDo: [
                {name: "Review code"},
                {name: "Test code"},
                {name: "Style code"},
                {name: "Help with code"}
            ],
            arrayInProgress: [],
            arrayDone: [],
            maxToDo: 5,
            maxInProgress: 5,
            maxDone: 5
        }
    },
    methods: {
        add() {
            if (this.newTask) {
                if (this.arrayToDo.length < this.maxToDo) {
                    this.arrayToDo.push({name: this.newTask});
                    this.newTask = "";
                } else {
                    alert("To Do limit has reached");
                }
            } else {
                alert("New Task cannot be empty");
            }
        },
        onDelete(arrayName, delTask) {
            var arr = null;
            if (arrayName == "arrayToDo") {
                arr = this.arrayToDo;
            } else if (arrayName == "arrayInProgress") {
                arr = this.arrayInProgress;
            } else {
                arr = this.arrayDone;
            }

            for (var i = 0; i < arr.length; i++) {
                var task = arr[i];
                if (task.name == delTask.name) {
                    arr.splice(i,1);
                }
            }
        },
        checkMove: function(event) {
            var to_array = null;
            var max = null;
            if (event.to.classList.contains("arrayToDo")) {
                to_array = this.arrayToDo;
                max = this.maxToDo;
            } else if (event.to.classList.contains("arrayInProgress")) {
                to_array = this.arrayInProgress;
                max = this.maxInProgress;
            } else {
                to_array = this.arrayDone;
                max = this.maxDone;
            }

            if (to_array.length == max) {
                return false;
            }
        },
        updateBoardTitle() {
            this.editBoardTitle = false;
        },
        updateBoardDescription() {
            this.editBoardDescription = false;
        }
    }
}
</script>

<style>
    .kanban-column {
        min-height: 300px;
    }
</style>
