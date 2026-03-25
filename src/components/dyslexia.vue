<script setup lang="ts">
import { onMounted } from 'vue';
let text = defineModel<string>();
let intervalID: number;
let delay = 1000;
let punc = ",.<>`~!@#$%^&*()[]{}\\|'\";:-_=+/? \n\r，。《》【】（）￥！’‘“”、；：＠＃＄％＾＆…";
let anim = true;

function update() {
    if (text.value) {
        text.value = randomShuffle(text.value)
    }
    let title = document.getElementById('title');
    if (title) {
        title.textContent = randomShuffle(title.textContent)
    }
    let tabTitle = document.getElementById('tab-title');
    if (tabTitle) {
        tabTitle.textContent = randomShuffle(tabTitle.textContent)
    }
}

function randomShuffle(str: string) {
    let word = ''
    let shuffledStr = ''
    for (let i = 0; i < str.length; i++) {
        if (!punc.includes(str[i])) {
            word += str[i]
        } else {
            shuffledStr += randomShuffleWord(word) + str[i]
            word = ''
        }
    }
    shuffledStr += randomShuffleWord(word)
    return shuffledStr
}

function randomShuffleWord(str: string) {
    if (str === '') {
        return ''
    }
    let arr = str.split('');
    let n = arr.length;
    for (let i = n - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        let tmp = arr[j];
        arr[j] = arr[i];
        arr[i] = tmp;
    }
    return arr.join('')
}

onMounted(function() {
    intervalID = setInterval(update, delay);
    anim = true;

    window.addEventListener('keydown', toggleAnim);

    function toggleAnim(e) {
        if (e.code !== 'Escape') {
            return
        }
        if (anim) {
            stopAnim();
            anim = false;
        } else {
            intervalID = setInterval(update, delay);
            anim = true;
        }
    }
})

function stopAnim() {
    clearInterval(intervalID);
}
</script>

<template>
    <div class="dyslexia">
        <h1 id="title">Dyslexia</h1>
        <textarea id="text" v-model="text" spellcheck="false">
        </textarea>
    </div>
</template>

<style scoped>
.dyslexia {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

#title {
    align-self: center;
}

#text {
    align-self: center;
    width: 80%;
    height: 80vh;
    outline: none;
    border: rgba(0, 0, 0, 0);
    resize: none;
    font-size: 20px;
    text-align: center;
}

#text:focus {
    outline: none;
    border: rgba(0, 0, 0, 0);
}
</style>