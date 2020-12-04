<template>
  <div class="user-input">
    <form class="user-input__form">
        <label for="input" class="user-input__input-label">Put username</label>
        <input v-model="username" id="input" type="text" class="user-input__input" />
        <button type="submit" class="user-input__submit" @click.prevent="activateRequest($event)">Submit</button>
    </form>
    <p class="user-input__error user-input__error--red">{{result}}</p>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop} from 'vue-property-decorator';
import { Octokit } from "@octokit/core";
import { Response } from '@/types';


@Component
export default class TheUserInput extends Vue {
    @Prop() private typeresp!: boolean;

    // Declaring OCTOKIT object for requests and USERNAME
    private octokit!: Octokit;
    private username = "";
    private resultMessage = "";

    private get result(): string {
        return this.resultMessage;
    }
    private set result(resultMessage: string) {
        this.resultMessage = resultMessage;
    }

    // Requesting for Repo data
    public activateRequest() {        
        const listOfRepos: Response[] = [];

        this.result = "Loading...";
        this.$emit("update-type-res", false);

        const response = this.octokit.request("GET /users/:org/repos?per_page=100", {
        org: this.username,
        type: "public",
        // perPage: 100,
        }).then((response) => {
            const repositories = response.data;
            // name, created_at, updated_at, description
            const propertyNames = ["name", "created_at", "updated_at", "description"];

            for(const repo of repositories) {
                const updatedRepo = Object.keys(repo)
                .filter((property) => propertyNames.includes(property))
                .reduce((obj: Response, key) => {
                    obj[key] = repo[key];
                    return obj;
                }, {});

                listOfRepos.push(updatedRepo);

                if (!this.typeresp) {
                    this.$emit("update-type-res", true);
                }
                this.result = "";
            }
            this.$emit("response", listOfRepos);
        }).catch(e => { 
            this.result = "User " + e.message;
            this.$emit("update-type-res", false);
        });
    }

    mounted() {
        // Creating octokit object for requests
        this.octokit = new Octokit({auth: `15babcf3ad9ee5a70cf915795e85d575cb6495da`})
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.user-input__error--red {
    color: red;
}

</style>
