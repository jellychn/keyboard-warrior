<template>
    <div class='container'>
    <div class='pragraph'>
        <div class='text-container' v-for='(block, b) in paragraph.split(" ")' :key='b'>
            <div class='block' v-for="(character, c) in block" :key="c">
                <p class='character' v-bind:class="{characterHover:index==String(b) + String(c)}" v-bind:id="String(b) + String(c)" v-bind:value="character">{{character}}</p>
            </div>
        </div>
    </div>
    <div class='keyboard'>
        <div class='column'>
            <div v-for="(keycap, x) in keyCaps0" :key="x">
                <Keycap v-if="keycap==this.pressedKey" v-bind:keycap="keycap" v-bind:pressed="true"/>
                <Keycap v-else v-bind:keycap="keycap" v-bind:pressed="false"/>
            </div>
        </div>
        <div class='column'>
            <div v-for="(keycap, x) in keyCaps1" :key="x">
                <Keycap v-if="keycap==this.pressedKey" v-bind:keycap="keycap" v-bind:pressed="true"/>
                <Keycap v-else v-bind:keycap="keycap" v-bind:pressed="false"/>
            </div>
        </div>
        <div class='column'>
            <div v-for="(keycap, x) in keyCaps2" :key="x">
                <Keycap v-if="keycap==this.pressedKey" v-bind:keycap="keycap" v-bind:pressed="true"/>
                <Keycap v-else v-bind:keycap="keycap" v-bind:pressed="false"/> 
            </div>
        </div>
        <!-- <div class='space' v-bind:class="{spacePressed:pressedKey==' '}"/> -->
    </div>
    </div>

</template>

<script>
import Keycap from './Keycap';

export default {
    name: 'Keybaord',
    created() {
        this.config();
        window.addEventListener('keydown', (e) => {
            this.registerKey(e.key)
        });
    },
    data() {
        return {
            pressedKey: '',
            keyCaps0: ['q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p'],
            keyCaps1: ['a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l'],
            keyCaps2: ['z', 'x', 'c', 'v', 'b', 'n', 'm'],

            index: "00",
            blockLength: 0,
            characterLength: 0,
            blockIndex: 0,
            characterIndex: 0,
            paragraph: '',
            completedList: new Set()
        }
    },
    components: {
        Keycap
    },
    methods: {
        config: function() {
            this.paragraph = Math.random().toString(36).replace(/[^a-z]+/g, '').substr(0,5);
            this.index = '00';
            this.blockIndex = 0;
            this.characterIndex = 0;
            this.blockLength = this.paragraph.split(' ').length - 1;
            this.characterLength = this.paragraph.split(' ')[0].length - 1;
        },
        keyPressSuccess: function() {
            this.completedList.add(this.index);
            if (this.characterLength === parseInt(this.index[1])) {
                this.characterIndex = 0;
                if (parseInt(this.index[0]) !== this.blockLength) { // check block length
                    this.blockIndex = this.blockIndex + 1;
                    this.index = String(this.blockIndex) + String(this.characterIndex);
                    this.characterLength = this.paragraph.split(' ')[this.blockIndex].length - 1;
                } else {
                    this.config();
                }
            } else {
                this.characterIndex += 1;
                this.index = String(this.blockIndex) + String(this.characterIndex);
            }
        },
        registerKey: function(key) {
            this.pressedKey = key;
            if (key == document.getElementById(this.index).innerHTML) {
                this.keyPressSuccess();
            } 
        }
    }
}
</script>

<style lang="postcss" scoped>
    .container {
        box-sizing: border-box;
        display: flex;
        align-items: center;
        flex-direction: column;
        height: 100vh;
    }
    .keyboard {
        box-sizing: border-box;
        border-radius: 5px;
        display: block;
        width: 80%;
        margin: 60px auto auto auto;
    }
    .column {
        display: flex;
        justify-content: center;
        margin: auto;
    }
    .space, .space-pressed {
        width: auto;
        margin: 20px auto 0 auto;
        height: 50px;
        border-radius: 3px;
    }
    .space {
        background-color: white;
    }
    .spacePressed {
        background-color: #ddd;
    }

    .pragraph {
        margin: auto auto 0 auto;
        display: block;
    }
    .text-container {
        margin: auto;
        width: 100px;
        overflow: auto;
    }
    .block {
        margin: 10px 5;
        float: left;
    }
    .character {
        font-family: 'Secular One', sans-serif;
        border-bottom: 3px solid white;
        margin: 0 1px;
        color: rgb(148, 148, 148);
        font-size: 20px;
    }
    .characterHover {
        border-bottom: 3px solid black;
    }
</style>