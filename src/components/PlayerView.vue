<template>
    <div>
        <audio ref="player"></audio>
        <div v-if="current.title">
            <h2>{{ current.title }}</h2>
            <h3>{{ current.artist }}</h3>
        </div>

        <button @click="previous">Previous</button>
        <button @click="play" v-if="!isPlaying">Play</button>
        <button @click="pause" v-else>Pause</button>
        <button @click="next">Next</button>
    </div>
</template>

<script>
import { ref, watchEffect } from 'vue';

    export default {
        name: 'PlayerView',
        props: {
            songs: {
                type: Array,
                required: true
            }
        },

        setup(props) {
            const current = ref({});
            const index = ref(0);
            var isPlaying = ref(false);
            const player = ref(new Audio());
        
            const play = () => {
                if (!isPlaying.value) {
                    const song = props.songs[index.value];
                    if (typeof song.src !== "undefined") {
                        current.value = song;
                        player.value.src = current.value.src;
                    }
                    player.value.play();
                }
            }

            const pause = () => {
                player.value.pause();
                isPlaying.value = false;
            }

            const next = () => {
                index.value++;
                if (index.value > props.songs.length - 1) {
                    index.value = 0;
                }

                current.value = props.songs[index.value];
                play();
            }

            const previous = () => {
                index.value--;
                if (index.value < 0) {
                    index.value = props.songs.length - 1;
                }

                current.value = props.songs[index.value];
                play();
            }

            watchEffect(() => {
                current.value = props.songs[index.value];
                player.value.src = current.value.src;
            });

            return {
                current,
                index,
                isPlaying,
                player,
                play,
                pause,
                next,
                previous
            };
        }
}
</script>