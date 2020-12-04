<template>
    <main class="repository">
        <section class="repository__title">
            {{this.repoName}}
        </section>
        <section class="repository__desc">
            {{this.repoDesc}}
        </section>
        <section class="repository__date-info">
            <p class="repository_created"><b>Created:</b> {{this.repoCreated}}</p>
            <p class="repository_updated"><b>Last Updated:</b> {{this.repoUpdated}}</p>
        </section>
        
    </main>
</template>

<script lang="ts">
import { Vue, Prop, Component } from 'vue-property-decorator';
import { Repo } from '@/types';

interface MonthsName {
    [index: string]: string;
}

@Component
export default class RepositoryFormat extends Vue {
    @Prop() private repository!: Repo;
    // Repo names, Date, Descriptions   

    private yearMatcher = /\b\d{4}\b/g;
    private monthMatcher= /\b\d{2}\b/;
    private dayMatcher = /\b\d{2}(?=T)/;

    private monthsName: MonthsName = {
        '01': 'Jan',
        '02': 'Feb',
        '03': 'Mar',
        '04': 'Apr',
        '05': 'May',
        '06': 'June',
        '07': 'July',
        '08': 'Aug',
        '09': 'Sept',
        '10': 'Oct',
        '11': 'Nov',
        '12': 'Dec',
    }



    get repoName() {
        return this.repository.name;
    }

    get repoDesc() {
        return this.repository.description;
    }

    get repoCreated() {
        const year = this.repository.created_at.match(this.yearMatcher)![0];
        const month = this.repository.created_at.match(this.monthMatcher)![0];
        const day = this.repository.created_at.match(this.dayMatcher)![0];
        // const result = this.repository.created_at.matchAll(this.resultMatcher);
        // return year[0] + month[0] + day[0];
        // console.log(result);
        // return year[0] + this.monthsName[month[0]] + day[0];
        // return month + ' ' + day + ' ' + year;
        return this.monthsName[month] + ' ' + day + ', ' + year;
    }

    get repoUpdated() {
        const year = this.repository.updated_at.match(this.yearMatcher)![0];
        const month = this.repository.updated_at.match(this.monthMatcher)![0];
        const day = this.repository.updated_at.match(this.dayMatcher)![0];
        
        return this.monthsName[month] + ' ' + day + ', ' + year;
    }

}
</script>

<style scoped lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro&display=swap');


@mixin changeFont($font) {
    font-family: $font, 'sans-serif';
}

.repository {
    padding: 1.5rem;
}

.repository__title {
    @include changeFont('Source Sans Pro');
    font-weight: bold;
}

.repository__desc {
    padding: 0.5rem 1rem;
}

.repository__date-info {
    margin-top: 3rem;
    text-align: left;
}


</style>
