

<script>
import axios from 'axios';

export default {
    name: 'Drive',
    data() {
        return {
            authenticated: false,
            videos: [],
            selected: ""
        }
    },
    mounted() {
        if (!document.cookie.includes("auth_user")) {
            this.$router.replace({ name: "login" });
        }
    },
    async created() {
        try {
            const res = await axios.get(`http://localhost:3001/videos`);
            if (res.status === 200) {
                this.videos = res.data;
            }
        } catch (e) {
            console.error(e);
        }
    },
    methods: {
        async selectVideo() {
            try {
                const video= this.videos.find(x => x.name === this.selected)
                const res = await axios.post(`http://localhost:3001/displayed`, video);

            } catch (e) {
                console.error(e)
            }

        }
    }

}
</script>
<template>

    <div>
        <h1>Piloter</h1>
        <select v-model="selected">
            <option disabled value="">choisir une vidéo</option>
            <option v-for="video in videos" v-bind:value="video.name">
                {{ video.name }}
            </option>
        </select>
        <button type="button" v-on:click="selectVideo()">Choisir {{ selected }}</button>

    </div>

</template>