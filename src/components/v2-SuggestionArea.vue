<template>
    <section>
        <section class="suggested-elements" v-show="availableOptions.length > 0" ref="suggestion_list">
            <ul>
                <li v-for="(option, index) in availableOptions" :key="option" @click="replaceText(option)" @mouseover="chooseCompletion($event)" :ref="`option_`+index">{{ option }}</li>
            </ul>
        </section>
        <textarea class="suggestion-area" v-model="currentText" @keydown.up="chooseCompletion($event)" @keydown.down="chooseCompletion($event)" @keydown.tab="autoComplete($event,
        currentSelectedOptionElement)" ref="suggestion_area_internal"></textarea> 
    </section>
</template>

<script>
export default {
    data() {
        return {
            currentText: "",
            showOptions: Boolean,
            filteredOptions: Array,
            resetOptions: Boolean,
            currentSelectedOption: 0,
            currentSelectedOptionElement: null
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
        },
        currentText(value) {
            this.$emit('input', value);
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
        autoComplete(event, currentSelectedElement) {
            event.preventDefault();
            if(this.availableOptions.length === 0) {
                return;
            }
            if(currentSelectedElement !== null) {
                this.replaceText(currentSelectedElement.innerHTML);
            } else {
                this.replaceText(this.availableOptions[0]);
            }
        },
        chooseCompletion(event) {
            event.preventDefault();
            if(this.currentSelectedOptionElement === undefined) {
                this.currentSelectedOptionElement = null;
            }
            if(event.code === "ArrowUp") {
                if(this.availableOptions.length === 0) {
                    return;
                }
                if(this.currentSelectedOption === 0 && this.currentSelectedOptionElement !== null) {
                    this.currentSelectedOption = this.availableOptions.length - 1;
                    this.currentSelectedOptionElement.classList.toggle('selected-option');
                }
                else if(this.currentSelectedOption === 0 && this.currentSelectedOptionElement == null) {
                    this.currentSelectedOption = 0;
                }
                else if(this.currentSelectedOptionElement !== null) {
                    this.currentSelectedOptionElement.classList.toggle('selected-option');
                    this.currentSelectedOption--;
                }
                this.currentSelectedOptionElement = this.$refs['option_' + this.currentSelectedOption][0];
                this.currentSelectedOptionElement.classList.toggle('selected-option');
                
                let section = this.$refs.suggestion_list;
                let offset = this.currentSelectedOptionElement.offsetTop;
                section.scrollTo({
                    top: 0 + offset,
                    behavior: 'smooth'
                })
            }
            else if(event.code === "ArrowDown") {
                if(this.availableOptions.length === 0) {
                    return;
                }
                if(this.currentSelectedOption === (this.availableOptions.length - 1) && this.currentSelectedOptionElement !== null) {
                    this.currentSelectedOption = 0;
                    this.currentSelectedOptionElement.classList.toggle('selected-option');
                }
                else if(this.currentSelectedOption === (this.availableOptions.length - 1)) {
                    this.currentSelectedOption = 0;
                }
                else if(this.currentSelectedOptionElement !== null) {
                    this.currentSelectedOptionElement.classList.toggle('selected-option');
                    this.currentSelectedOption++;
                }
                this.currentSelectedOptionElement = this.$refs['option_' + this.currentSelectedOption][0];
                this.currentSelectedOptionElement.classList.toggle('selected-option');

                let section = this.$refs.suggestion_list;
                let offset = this.currentSelectedOptionElement.offsetTop;
                section.scrollTo({
                    top: 0 + offset,
                    behavior: 'smooth'
                })
            } else if(event.type === "mouseover") {
                if(this.currentSelectedOptionElement !== null) {
                    this.currentSelectedOptionElement.classList.toggle('selected-option');
                }
                this.currentSelectedOptionElement = event.target;
                this.currentSelectedOptionElement.classList.toggle('selected-option');
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

.selected-option {
    background-color: #2552F2;
    color: white;
}

</style>