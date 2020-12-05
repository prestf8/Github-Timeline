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

<style scoped>

* {
    margin: 0;
    padding:0;
}

.timeline {
    margin-top: 3rem;
}

.timeline__length {
    font-weight: bold;
}

.timeline__repos {
    /* background-color: lightcoral; */
    max-width: 900px;
    margin: 0 auto;
    
    position: relative;
}

.timeline__repos:after {
    content: '';
    position: absolute;
    top:0;
    bottom:0;
    left: 50%;
    transform: translate(-50%);
    width: 8px;
    background-color: lightgreen;
}

.timeline__repo-li {
    list-style: none;
    width: 50%;
    height: 270px;
    margin-top: 1rem;
    border: none;
    border-radius: 5px;
    padding: 2rem;

    position: relative;
}

.timeline__repo-li:after {
    position: absolute;
    content: ' ';
    width: 20px;
    height: 20px;
    border: 5px solid lightgreen;
    border-radius: 50%;
    background-color: white;
    z-index: 1;
}

.left {
    left: 0%;
    /* transform: translateX(-50%); */
}

.left:after {
    top: 1rem;
    right:0;
    transform: translateX(50%); 
}

.right {
    left: 50%; 
    /* transform: translateX(-50%); */
}

.right:after {
    top: 1rem;
    left:0;
    transform: translateX(-50%);
}

@media screen and (max-width: 700px) {
    .timeline__repos:after {
        left: 1rem;
        transform: translateX(0);
    }

    .timeline__repo-li {
        left: 0;
        width: 100%;
        padding:3rem;
    }

    .timeline__repo-li:after {
        left: 0;
        top: 3rem;
        transform: translateX(0);
        margin-left: 4px;
    }
}
</style>