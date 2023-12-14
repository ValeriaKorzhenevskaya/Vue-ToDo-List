<script>
export default {
    name: 'TodoFooter',

    props: ['remaining', 'visibility', 'todos'],
    emits: ['removeCompleted'],

    methods: {
        removeCompleted() {
            this.$emit('removeCompleted');
        }
    }
}
</script>

<template>
    <footer class="footer" v-show="todos.length">
        <span class="todo-count">
            <strong>{{ remaining }}</strong>
            <span>{{ remaining === 1 ? ' item' : ' items' }} left</span>
        </span>
        <ul class="filters">
            <li>
                <a href="#/all" :class="{ selected: visibility === 'all' }">All</a>
            </li>
            <li>
                <a href="#/active" :class="{ selected: visibility === 'active' }">Active</a>
            </li>
            <li>
                <a href="#/completed" :class="{ selected: visibility === 'completed' }">Completed</a>
            </li>
        </ul>
        <button class="clear-completed" @click="removeCompleted" v-show="todos.length > remaining">
            Clear completed todos
        </button>
    </footer>
</template>

<style scoped>
.footer {
    display: flex;
    justify-content: space-between;
    padding: 10px 15px;
    height: 20px;
    text-align: center;
    font-size: 15px;
    border-top: 1px solid #e6e6e6;
}

/* center */
.filters {
    margin: 0;
    padding: 0;
}
.filters li {
    display: inline;
}

.filters li a {
    color: inherit;
    margin: 3px;
    padding: 3px 7px;
    text-decoration: none;
    border: 1px solid transparent;
    border-radius: 3px;
}
.filters li a:hover {
    border-color: #DB7676;
}
.filters li a.selected {
    border-color: #CE4646;
}
/* clear completed */
.clear-completed,
.clear-completed:active {
    position: relative;
    cursor: pointer;
}
.clear-completed:hover {
    text-decoration: underline;
}
</style>
