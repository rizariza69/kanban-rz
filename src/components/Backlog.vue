<template>
    <div class="backlog-view">
        <new-item></new-item>

        <div class="card" v-for="item in items" :key="item.id">
            <div class="card-block">
                <h5 class="card-title">
                    <span class="text-muted">#{{item.id}}</span>
                    {{item.text}}
                    <span :class="badgeClass(item)">{{badgeText(item)}}</span>
                </h5>
            </div>
        </div>
    </div>
</template>

<script>
    const badgeDetail = {
        todo: {
            text: 'to-do',
            class: 'badge badge-light',
        },
        inProgress: {
            text: 'in progress',
            class: 'badge badge-info',
        },
        done: {
            text: 'done',
            class: 'badge badge-success',
        },
    };



    import {
        mapState
    } from 'vuex';
    import NewItemForm from './NewItemForm';

    export default {
        name: 'Backlog',
        components: {
            'new-item': NewItemForm,
        },
        methods: {
            itemLane(item) {
                if (this.$store.state.items.todo.includes(item)) {
                    return 'todo';
                } else if (this.$store.state.items.inProgress.includes(item)) {
                    return 'inProgress';
                }

                return 'done';
            },
            badgeText(item) {
                const lane = this.itemLane(item);
                return badgeDetail[lane].text;
            },
            badgeClass(item) {
                const lane = this.itemLane(item);
                return badgeDetail[lane].class;
            },
        },
        computed: mapState({
            items: s => [...s.items.todo, ...s.items.inProgress, ...s.items.done]
        }),
    };
</script>

<style>
    .card {
        margin-bottom: 10px;
    }

    .card:last-child {
        margin-bottom: 25px;
    }

    .card-block {
        padding: 15px;
    }

    .add-item {
        margin: 30px 0;
    }
</style>