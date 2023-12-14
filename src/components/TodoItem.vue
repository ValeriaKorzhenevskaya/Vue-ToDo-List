<script>
export default {
    name: 'TodoItem',
    
    props: ['filteredTodos', 'editedTodo'],

    emits: ['editTodo', 'removeTodo', 'cancelEdit', 'doneEdit'],

    methods: {
        editTodo(todo) {
            this.$emit('editTodo', todo);
        },
        removeTodo(todo) {
            this.$emit('removeTodo', todo);
        },
        cancelEdit(todo) {
            this.$emit('cancelEdit', todo);
        },
        doneEdit(todo) {
            this.$emit('doneEdit', todo);
        }
    }
}
</script>

<template>
    <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item" :class="{ completed: todo.completed, editing: todo === editedTodo }">
        <div class="view">
            <input class="toggle" type="checkbox" v-model="todo.completed">
            <label @dblclick="editTodo(todo)">{{ todo.title }}</label>
            <button class="destroy" @click="removeTodo(todo)"></button>
        </div>

        <input
            @vnode-mounted="({ el }) => el.focus()"
            v-if="todo === editedTodo"
            class="edit"
            type="text"
            v-model="todo.title"
            @keyup.escape="cancelEdit(todo)"
            @blur="doneEdit(todo)"
            @keyup.enter="doneEdit(todo)"
        >
    </li>
</template>

<style scoped>
    /* todo list items and states */
.todo-list li {
    position: relative;
    font-size: 24px;
    border-bottom: 1px solid #ededed;
}
.todo-list li:last-child {
    border-bottom: none;
}
.todo-item.editing {
    border-bottom: none;
    padding: 0;
}

.todo-item.editing .edit {
    display: block;
    width: calc(100% - 43px);
    padding: 12px 16px;
    margin-left: 43px;
}
.todo-item label {
    word-break: break-all;
    padding: 15px 15px 15px 60px;
    display: block;
    color: #484848;
}
.todo-item.completed label {
    color: #949494;
    text-decoration: line-through;
}
.todo-item .edit {
    display: none;
}
.todo-item.editing .view {
    display: none;
}
.todo-item .destroy {
    display: none;
    position: absolute;
    cursor: pointer;
    top: 5px;
    right: 10px;
    width: 40px;
    height: 40px;
    font-size: 30px;
    color: #949494;
}
.todo-item .destroy:hover,
.todo-item .destroy:focus {
    color: #C18585;
}
.todo-item .destroy:after {
    content: '×';
}
.todo-item:hover .destroy {
    display: block;
}
/* edit which hides checkbox */
.todo-item .toggle {
    position: absolute;
    top: 50%;
    transform: translate(50%, -50%);
    margin: auto 0;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    cursor: pointer;
}

.todo-item .toggle + label {
    background-image:
    url('data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23949494%22%20strokewidth%3D%223%22/%3E%3C/svg%3E');
    background-repeat: no-repeat;
    background-position: center left;
}
/* circle checked - no checkmark when we click it! */
.todo-item .toggle:checked + label {
    background-image:
    url('data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%2359A193%22%20strokewidth%3D%223%22%2F%3E%3Cpath%20fill%3D%22%233EA390%22%20d%3D%22M72%2025L42%2071%2027%2056l-4%204%2020%2020%2034-52z%22%2F%3E%3C%2Fsvg%3E');
}

</style>
