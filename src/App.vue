<script>

import TodoItem from './components/TodoItem.vue';
import TodoFooter from './components/TodoFooter.vue';

const filters = {
    all: (todos) => todos,
    active: (todos) => todos.filter((todo) => !todo.completed),
    completed: (todos) => todos.filter((todo) => todo.completed)
};

export default {
    name: 'TodoApp',

    components: {
        TodoItem,
        TodoFooter
    },
    
    // app initial state
    data: () => ({
        todos: JSON.parse(localStorage.getItem('todoappData') || '[]'),
        visibility: 'all',
        beforeEditCancel: null,
        editedTodo: null
    }),

    // watch todos change for localStorage persistence
    watch: {
        todos: {
            handler(todos) {
                localStorage.setItem('todoappData', JSON.stringify(todos))
            },
            deep: true
        }
    },

    mounted() {
        window.addEventListener('hashchange', this.onHashChange)
        this.onHashChange()
    },

    computed: {
        remaining() {
            return this.todos.length;
        },
        filteredTodos() {
            return filters[this.visibility](this.todos);
        }
    },

    // methods that implement data logic
    methods: {
        onHashChange() {
            var visibility = window.location.hash.replace(/#\/?/, '');
            if (filters[visibility]) {
                this.visibility = visibility;
            } else {
                window.location.hash = '';
                this.visibility = 'all';
            }
        },
        toggleAll(e) {
            this.todos.forEach((todo) => (todo.completed = e.target.checked))
        },
        removeCompleted() {
            this.todos = filters.active(this.todos)
        },
        addTodo(event) {
            // TODO: Add code here.
            const value = event.target.value.trim();

            if (!value) {
                return;
            }

            this.todos.push({
                id: Date.now(),
                title: value,
                isCompleted: false
            });

            event.target.value = ''
        },
        removeTodo(todo) {
            this.todos.splice(this.todos.indexOf(todo), 1);
        },
        editTodo(todo) {
            this.beforeEditCancel = todo.title;
            this.editedTodo = todo;
        },
        cancelEdit(todo) {
            this.editedTodo = null
            todo.title = this.beforeEditCancel
        },
        doneEdit(todo) {
            this.editedTodo = null;
            todo.title = todo.title.trim();
            
            if (!todo.title) {
                this.removeTodo(todo);
            }
        }
    }
}
</script>

<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todos</h1>
            <input autofocus class="new-todo" placeholder="What needs to be done?" @keyup.enter="addTodo">
        </header>
        <section class="main" v-show="todos.length">
            <input class="toggle-all" id="toggle-all" type="checkbox" :checked="remaining === 0" @change="toggleAll">
            <label for="toggle-all">Mark all as complete</label>
            <ul class="todo-list">
                <TodoItem 
                    :filteredTodos="filteredTodos"
                    :editedTodo="editedTodo"
                    @editTodo="editTodo"
                    @removeTodo="removeTodo"
                    @cancelEdit="cancelEdit"
                    @doneEdit="doneEdit"
                />
            </ul>
        </section>
        <TodoFooter
            :remaining="remaining"
            :visibility="visibility"
            :todos="todos"
            @removeCompleted="removeCompleted"
        />
    </section>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.header {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

/* body */
body {
    font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
    line-height: 1.4em;
    background: #f5f5f5;
    color: #111111;
    margin: 0 auto;
}
/* button */
button {
    border: none;
    background: none;
}
/* .todoapp */
.todoapp {
    background: #fff;
    position: relative;
    padding: 20px;
}
/* h1 */
.todoapp h1 {
    position: absolute;
    top: -140px;
    width: 100%;
    font-size: 80px;
    font-weight: 200;
    text-align: center;
    color: #b83f45;
}
/* placeholder */
.todoapp input::placeholder {
    font-style: italic;
    color: rgb(0, 0, 0);
}
/* input::new/edit */
.new-todo,
.edit {
    width: 100%;
    font-size: 24px;
    box-sizing: border-box;
}
.new-todo {
    padding: 16px 16px 16px 60px;
    border: none;
}
/* main, toggle-all */
/* relative aligns the new-todo input and down-facing chevron */
.main {
    position: relative; 
}
.toggle-all {
    opacity: 0;
    position: absolute;
}
.toggle-all + label {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 45px;
    height: 65px;
    font-size: 0;
    position: absolute;
    top: -65px;
}
.toggle-all + label:before {
    content: '❯';
    font-size: 22px;
    color: #949494;
    padding: 10px 27px 10px 27px;
    transform: rotate(90deg);
    cursor: pointer;
}
.toggle-all:checked + label:before {
    color: #484848;
}
/* todo-list */
.todo-list {
    margin: 0;
    padding: 0;
    list-style: none;
}

</style>