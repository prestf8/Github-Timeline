<template>
  <div class="user-input">
    <form class="user-input__form">
        <!-- <label for="input" class="user-input__input-label">Put username</label> -->
        <input v-model="username" id="input" type="text" class="user-input__input" spellcheck="false"/>
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
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro&display=swap');
$vue-color-one: #009c63;
// $vue-color-two: #163f61;
$input-background: #353535;
$button-color: #f54949;

@mixin displayFlex($j, $a, $direction, $typeofWrap) {
    display: flex;
    justify-content: $j;
    align-items: $a;
    flex-flow: $typeofWrap $direction;
}

@mixin changeFont($font) {
    font-family: $font, 'sans-serif';
}

.user-input__input:-webkit-autofill,
.user-input__input:-webkit-autofill:hover, 
.user-input__input:-webkit-autofill:focus
 {
//   border: 1px solid green;
  -webkit-text-fill-color: white;
   box-shadow: 0 0 0px 1000px $input-background inset;
  -webkit-box-shadow: 0 0 0px 1000px $input-background inset;
  transition: background-color 5000s ease-in-out 0s;
    caret-color: white;
}

.user-input__input:-webkit-autofill::first-line {
    font-size: 0.9rem;
    @include changeFont('Source Sans Pro');
}

.user-input__form {
    @include displayFlex(center, center, column, nowrap);
}



// .user-input__input-label {
//     @include changeFont('Source Sans Pro');
// }

.user-input__input {
    max-width: 400px;
    outline: none;
    width: 80%;
    height: 30px;
    padding: 3px;
    border: none;
    // border: 1px solid white;
    border-radius: 2px;
    color: white;
    background-color: $input-background;
    margin-bottom: 0.3rem;
    font-size: 0.9rem;
    @include changeFont('Source Sans Pro');
}

.user-input__submit {
    max-width: 400px;
    width: 80%;
    height: 35px;
    color: white;
    background-color: $vue-color-one;
    border-radius: 2px;
    border: none;
    outline: none;
    @include changeFont('Source Sans Pro');

}

.user-input__submit:hover {
    cursor: pointer;
}

// Error message / Status Message
.user-input__error--red {
    margin-top: 2rem;
    color: $button-color;
    font-size: 2rem;
    @include changeFont('Source Sans Pro');
}

@media screen and (min-width: 600px) {
    .user-input__input {
        height: 40px;
        font-size: 1.2rem;
        padding: 5px;
    }

    .user-input__input:-webkit-autofill::first-line {
        font-size: 1.2rem;
    }

    .user-input__submit {
        height: 40px;
        font-size: 1.2rem;
    }
}


</style>
