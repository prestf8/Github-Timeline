<template>
    <div class="timeline">
        <p class="timeline__length">{{this.reposLength}} Repositories (100 Max)</p>
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
    background-color: lightskyblue;
}

.timeline__repos::after {
    content: '';
    position: absolute;
    top:0;
    bottom:0;
    left: 50%;
    transform: translate(-50%);


    width: 5px;
    background-color: black;
}

.timeline__repo-li {
    list-style: none;
    width: 35%;
    min-height: 150px;

    margin-top: 1rem;
    border: 1px solid black;
    position: relative;
    
}

.right {
    background-color: green;
    left: 25%;
    transform: translateX(-50%);
}

.left {
    background-color: blue;
    left: 75%;
    transform: translateX(-50%);

}
</style>