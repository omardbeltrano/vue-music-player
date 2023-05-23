<template>
    <div class="container">
        <audio ref="player"></audio>
        <section class="player">
            <img :src="current.img" alt="">
            <h2 class="title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
            <div class="controls">
                
                <awesome class="prev" @click="previous" icon="fa-solid fa-backward" />
                <awesome class="play" @click="play" v-if="!isPlaying" icon="fa-regular fa-circle-play" />
                <awesome class="pause" @click="pause" v-else icon="fa-regular fa-circle-pause" />
                <awesome class="next" @click="next" icon="fa-solid fa-forward" />
            </div>
            <div class="song-timer">
                <span>{{ formattedCurrentTime }}</span> / {{ formattedDuration }}
            </div>
        </section>
        <section class="playlist">
            <button :class="(song.src == current.src) ? 'song playing' : 'song'" v-for="song in songs" :key="song.src" @click="play(song)">
                {{ song.title }} - {{ song.artist }}
            </button>
        </section>
    </div>
</template>

<script>
    export default {
        name: 'PlayerSongs',
        data(){
            return {
                current: {},
                index: 0,
                isPlaying: false,
                songs: [
                    {
                    title: "Too Little Too Late",
                    artist: "A Skylit Drive",
                    img: require('../assets/asd.jpg'),
                    src: require('../assets/Too Little Too Late.mp3')
                    },
                    {
                    title: "Crow Song",
                    artist: "Lisa",
                    img: require('../assets/angelsBeats.jpg'),
                    src: require('../assets/Crow Song.mp3')
                    },
                    {
                    title: "Alchemy",
                    artist: "Lisa",
                    img: require('../assets/angelsBeats.jpg'),
                    src: require('../assets/Alchemy.mp3')
                    },
                    {
                    title: "My Song",
                    artist: "Lisa",
                    img: require('../assets/angelsBeats.jpg'),
                    src: require('../assets/My Song.mp3')
                    },
                    {
                    title: "Waking the Demon",
                    artist: "Bullet for my Valentine",
                    img: require('../assets/bfmv.jpg'),
                    src: require('../assets/Waking The Demon.mp3')
                    },
                    {
                    title: "Mr. Highways Thinking About the End",
                    artist: "A Day to Remember",
                    img: require('../assets/adtr.jpg'),
                    src: require('../assets/mr. highways thinking about the end.mp3')
                    },
                    {
                    title: "Answer Song",
                    artist: "Lisa",
                    img: require('../assets/ab.jpg'),
                    src: require('../assets/Answer Song.mp3')
                    },
                    {
                    title: "Little Braver",
                    artist: "Lisa",
                    img: require('../assets/ab.jpg'),
                    src: require('../assets/Little Braver.mp3')
                    }
                ],
                player: new Audio(),
                currentTime: 0,
                duration: 0,
            }
        },
        computed: {
            formattedCurrentTime() {
            return this.formatTime(this.currentTime);
            },
            formattedDuration() {
            return this.formatTime(this.duration);
            },
        },
        methods: {
            play(song) {
                if (typeof song.src != "undefined") {
                    this.current = song;
                    this.player.src = this.current.src;
                }

                this.player.play();
                this.player.addEventListener('ended', function() {
                    this.index++;
                    if (this.index > this.songs.length - 1) {
                        this.index = 0;
                    }

                    this.current = this.songs[this.index];
                    this.play(this.current);
                }.bind(this));

                this.isPlaying = true;
            },

            pause() {
                this.player.pause();
                this.isPlaying = false;
            },

            next() {
                this.index++;
                if (this.index > this.songs.length - 1) {
                    this.index = 0;
                }

                this.current = this.songs[this.index];
                this.play(this.current);
            },

            previous() {
                this.index--;
                if (this.index < 0) {
                    this.index = this.songs.length - 1;
                }

                this.current = this.songs[this.index];
                this.play(this.current);
            },

            updateTime() {
                this.currentTime = this.player.currentTime;
                this.duration = this.player.duration;
            },

            formatTime(time) {
                const minutes = Math.floor(time / 60);
                const seconds = Math.floor(time % 60).toString().padStart(2, "0");
                return `${minutes}:${seconds}`;
            },
            
        },

        created() {
            this.current = this.songs[this.index];
            this.player.src = this.current.src;
            this.player.addEventListener('timeupdate', this.updateTime);
        }
    }
</script>

<style lang="scss" scoped>
*{
    color: #FFF;
    button {
            appearance: none;
            background: none;
            border: none;
            outline: none;
            cursor: pointer;
        }

    button:hover {
        opacity: 0.8;
    }
}

.container{
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    border-radius: 8px;
    padding: 15px;
    box-shadow: 0px 3px 6px rgba(0,0,0,0.4);
    background-image: linear-gradient(to top right, #212121, #575757);
    .player {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-bottom: 20px;
        img {
            margin-top: 14px;
            margin-bottom: 25px;
            width: 240px;
            border: 5px solid #FFF;
            border-radius: 4px;
        }
        .title {
            //color: #53565A;
            //font-size: 24px;
            font-size: 1rem;
            font-weight: 700;
            //text-transform: uppercase;
            text-align: center;
        }

        .title span {
            font-weight: 400;
        }

        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 30px 15px;

            .play {
                color: #FFF;
                &:hover {
                    color: #b7b7b7;
                }
            }

            .pause {
                color: #CC2E5D;

                &:hover {
                    color: #b42f57;
                }
            }
            .play, .pause {
                width: 100%;
                display: block;
                font-size: 40px;
                font-weight: 700;
                padding: 5px 15px;
                margin: 0px 15px;
                border-radius: 8px;
                cursor: pointer;

                &:hover {
                    transform: scale(1.2);
                }
            }

            .prev, .next {
                width: 100%;
                font-size: 40px;
                font-weight: 700;
                padding: 5px 10px;
                margin: 0 15px;
                border-radius: 6px;
                color: #FF5858;
                cursor: pointer;

                &:hover {
                    color: #c74545;
                    transform: scale(1.2);
                }
            }
        }
    }

    .playlist {
        padding: 0px 30px;
        width: 100%;
        border-radius: 8px;
        padding: 10px;
        //height: 220px;
        overflow-y: auto;

        &:hover {
            box-shadow: inset 0px 0px 15px rgba(0, 0, 0, 0.15);
            background-image: linear-gradient(to bottom right, #212121, #575757);
        }
        /* Customize the scroll bar */
        h3 {
            //color: #212121;
            font-size: 28px;
            font-weight: 400;
            margin-bottom: 30px;
            text-align: center;
        }

        .song {
            display: block;
            width: 90%;
            padding: 10px;
            font-size: 14px;
            font-weight: 700;
            border-radius: 8px;
            margin: auto;

            &:hover {
            color: #ff5858;
            }

            &.playing {
                color: #fff;
                background-image: linear-gradient(to right, #cc2e5d, #ff5858);
            }
        }
    }

    .playlist::-webkit-scrollbar{
            width: 4px;
            scrollbar-width: thin;
        }

        /* Track */
        .playlist::-webkit-scrollbar-track {
            background-color: #212121;
        }

        /* Handle */
        .playlist::-webkit-scrollbar-thumb {
            background-color:  #FF5858;
            border-radius: 8px;
            
        }

        /* Handle on hover */
        .playlist::-webkit-scrollbar-thumb:hover {
            background-color: #CC2E5D;
        }
}
</style>