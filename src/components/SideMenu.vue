<template>
    <v-ons-splitter style="marginTop:7.1vh;z-index:10000;pointer-events:none;position:fixed;">
        <v-ons-splitter-side style="pointer-events:auto;"
            swipeable width="" collapse="" side="right"
            :open.sync="$store.state.openSide">
            <v-ons-page>
            <v-ons-list>
                <v-ons-list-item v-for="(page,iter) in pages" style="height:10vh;"
                    tappable modifier="chevron" v-bind:key=iter
                    @click="changePage(page[1])">
                    <div class="center">{{page[0]}}</div>
                </v-ons-list-item>
            </v-ons-list>
            </v-ons-page>
        </v-ons-splitter-side>        
    </v-ons-splitter>
</template>

<script>
export default {
    data() {
        return {
            pages: [ // Label and page index
                ["Watched Movies", 3],
                ["To-Watch List", 4],
                ["My Favorites", 6],
                ["Recommendations", 5],
                ["Hot Movies", 7],
                ["In Theaters", 8],
                ["Settings", 2]
            ],
        }
    },
    methods: {
        changePage(page) {
            this.$store.commit('toggleSideSplitter');
            if (this.$store.state.crntPage != page) {
                this.$store.commit('pushPageStack', page);
            }
        }
    }
}
</script>