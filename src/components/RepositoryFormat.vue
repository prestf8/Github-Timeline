<template>
    <main class="repository">
        <section class="repository__title">
            <p class="repository__title-content">{{this.repoName}}</p>
        </section>
        <section class="repository__desc">
            <p class="repository__desc-content">{{this.repoDesc}}</p>
        </section>
        <section class="repository__date-info">
            <p class="repository-date-info repository-date-info--created"><b>Created:</b> {{this.repoCreated}}</p>
            <p class="repository-date-info repository-date-info--updated"><b>Last Updated:</b> {{this.repoUpdated}}</p>
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
@import url('https://fonts.googleapis.com/css2?family=Poppins&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Nanum+Gothic&display=swap');
$vue-color-one: #009c63;
$vue-color-two: #163f61;

@mixin changeFont($font) {
    font-family: $font, 'sans-serif';
}

@mixin displayFlex($j, $a, $direction, $typeofWrap) {
    display: flex;
    justify-content: $j;
    align-items: $a;
    flex-flow: $typeofWrap $direction;
}

.repository {
    @include displayFlex(stretch, stretch, column, nowrap);
    height: 100%;
    width: 100%;
    background: lightgreen;
    border: 8px solid $vue-color-one;
    border-radius: 5px;
    padding: 0.5rem;
    min-height: 150px;
}

.repository__title {
    font-weight: bold;
    padding: .5rem;
    flex: 1;
    @include changeFont('Source Sans Pro');
    @include displayFlex(center, center, row, nowrap);
}

.repository__desc {
    font-size: 0.9rem;
    padding: 0.5rem 1rem;
    flex: 1;
    @include changeFont('Poppins');
    @include displayFlex(center, center, row, nowrap);
}

.repository__date-info {
    display: none;
    text-align: left;
    margin-left: 1rem;
    margin-bottom: 1rem;
    margin-top: 2rem;
    flex: 2;
    @include changeFont('Nanum Gothic');
}

@media screen and (min-width: 500px) {
    .repository__date-info {
        display: block;
        @include displayFlex(flex-end, flex-start, nowrap, column)
    }

    .repository__title {
        font-size: 1.2rem;
        margin-top: 1rem;
    }
}

@media screen and (min-width: 1200px) {
    .repository__title {
        font-size: 1.4rem;
    }
}


</style>
