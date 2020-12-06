<template>
    <div class="timeline">
        <p class="timeline__length">{{this.reposLength}} Public Repositories (100 Max)</p>
        <ul class="timeline__repos">
            <li class="timeline__repo-li" :class="index%2==0 ? 'right' : 'left'" v-for="(repo, index) in this.repoData" :key="repo.name">
                <RepositoryFormat :repository="repo"></RepositoryFormat>
            </li>
        </ul>
    </div>
</template>

<script lang="ts">
//Timeline: False => Timeline: True
import { Vue, Prop, Component } from 'vue-property-decorator';
import RepositoryFormat from './RepositoryFormat.vue'

@Component({
    components: {
        RepositoryFormat,
    }
})
export default class Timeline extends Vue {
    @Prop() private repoData!: Array<object>;

    get reposLength() {
        return this.repoData.length;
    }

// Repo names, Date, Descriptions 
}
</script>

<style scoped lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Nanum+Gothic&display=swap');

$vue-color-one: #009c63;
$vue-color-two: #1a4b73;
$temp-color: #353535;

* {
    margin: 0;
    padding:0;
}

.timeline {
    margin-top: 3rem;
    padding: 1rem;
    background-color: $temp-color;
}

.timeline__length {
    /* float: right; */
    margin: 1rem;
    color: white;

    font-family: 'Nanum Gothic', sans-serif;
}

.timeline__repos {
    /* background-color: lightcoral; */
    max-width: 900px;
    margin: 0 auto;
    position: relative;
    background-color: inherit;
    border: none;
}

.timeline__repos:after {
    content: '';
    position: absolute;
    top:0;
    bottom:0;
    width: 8px;
    background-color: $vue-color-one;

    left: 1rem;
    transform: translateX(0);
}

.timeline__repo-li {
    list-style: none;
    width: 100%;
    margin-top: 1rem;
    border: none;
    border-radius: 5px; 
    padding: 3rem;
    min-height: 200px;

    position: relative;
}

.timeline__repo-li:after {
    position: absolute;
    content: ' ';
    width: 20px;
    height: 20px;
    border: 5px solid $vue-color-one;
    border-radius: 50%;
    background-color: white;
    z-index: 1;

    left: 0;
    top: 3rem;
    transform: translateX(0);
    margin-left: 4px;
}

@media screen and (min-width: 600px) {

    .timeline__length {
        font-size: 1.2rem;
    }

    .timeline__repos {
        border: 6px solid $vue-color-one;

    }

    .timeline__repos:after {
        left: 50%;
        transform: translate(-50%);
    }

    .timeline__repo-li {
        width: 50%;
        padding: 2rem;
    }

    .timeline__repo-li:after {
        margin-left: 0;
        left: 50%;
        transform: translateX(-50%);
    }
    
    .left {
        left:0;
    }

    .left:after {
        left: 100%;
    } 

    .right {
        left: 50%;
    }

    .right:after {
        left:0;
        /* transform: translateX(50%); */
    }
}
</style>