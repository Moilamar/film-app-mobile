<template>
    <v-ons-page>
        <basic-info :movie="movie"></basic-info>
    </v-ons-page>
</template>

<script>
import basicInfo from './BasicInfo';


export default {
    data() {
        return {

        }
    },
    methods: {
        getReviews() {
            
        }
    },
    props: [ 'movie' ],
    components: { basicInfo }
}
</script>