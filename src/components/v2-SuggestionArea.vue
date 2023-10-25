<template>
    <section>
        <section class="suggested-elements" v-show="availableOptions.length > 0">
            <ul>
                <li v-for="option in availableOptions" :key="option" @click="replaceText(option)">{{ option }}</li>
            </ul>
        </section>
        <textarea class="suggestion-area" v-model="currentText" @keydown.up="chooseCompletion()" @keydown.tab="autoComplete($event)" ref="suggestion_area_internal"></textarea> 
    </section>
</template>

<script>
export default {
    data() {
        return {
            currentText: "",
            showOptions: Boolean,
            filteredOptions: Array,
            selected_option: null,
            resetOptions: Boolean
        }
    },
    props: {
        options: Array
    },
    mounted() {
        this.currentText = "";
        this.showOptions = false;
        this.filteredOptions = this.options;
        this.showOptions = true;
        this.resetOptions = false;
    },
    watch: {
        availableOptions(value) {
            if(value.length > 0)
                this.showOptions = true;
            else
                this.showOptions = false;
            this.showOptions = true;
        }
    },
    computed: {
        availableOptions() {
            let availableOptions = []
            if(this.currentText === "") {
                availableOptions = [];
            } else if(this.resetOptions) {
                availableOptions = [];
                this.resetOptions = false;
            }
            else {
                let words = null;
                words = this.currentText.split(' ');
                let finalWord = words[words.length - 1];
                
                if(finalWord !== '')
                    availableOptions = this.options.filter(option => option.substring(0, finalWord.length) === finalWord);
            }
            return availableOptions;
        }
    },
    methods: {
        replaceText(value) {
            let textarea = this.$refs.suggestion_area_internal;
            let fullText = this.currentText;
            let splittedText = null;

            splittedText = fullText.split(' ');
            splittedText[splittedText.length - 1] = value;
            
            this.currentText = splittedText.join(' ');
            this.resetOptions = true;
            textarea.focus();
            this.currentText += ' ';
        },
        autoComplete(event) {
            event.preventDefault();
            if(this.availableOptions.length > 0) {
                this.replaceText(this.availableOptions[0]);
            }
        }
    }
}
</script>

<style scoped>
.suggestion-area {
    width: 300px;
    height: 100px;
    display: inline;
}

.suggested-elements {
    position: absolute;
    top: 180px;
    height: 120px;
    overflow: auto;
    width: 200px;
    border: 1px solid black;
}

.suggested-elements ul {
    margin: 0;
    padding: 0;
}
.suggested-elements ul li {
    list-style: none;
    padding: 5px 0;
    padding-left: 10px;
    cursor: pointer;
}

.suggested-elements ul li:hover {
    background-color: #2552F2;
    color: white;
}

</style>