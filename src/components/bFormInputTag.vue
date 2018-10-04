<template>
    <div class="vue-input-tag-wrapper">
        <span v-for="(tag, index) in tags" :key="index" class="input-tag">
            <span>{{ tag }}</span>
            <span @click="remove(index)" class="remove"> x</span>
        </span>
    <input v-if="!isLimit" ref="input" :placeholder="placeholder" type="text" v-model="newTag" class="new-tag" @keydown="checkTag"/>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                tags: this.value.split(this.separators).map(s => s.trim()),
                newTag: ""
            }
        },
        props: {
            value: {
                type: String,
                default: ''
            },
            placeholder: {
                type: String,
                default: 'Input Tag'
            },
            separators: {
                type: String,
                default: ','
            },
            limit: {
                type: Number,
                default: -1
            }
        },
        computed: {
            isLimit() {
                return this.limit > 0 && Number(this.limit) < this.tags.length
            }
        },
        watch: {
            value() {
                this.tags = this.value.split(this.separators).map(tag => tag.trim()).filter(tag => tag != '')
            }
        },
        methods: {
            tagChange() {
                this.$emit("input", this.tags.toString())
            },

            remove(index) {
                this.tags.splice(index, 1)
                this.tagChange()
            },

            checkTag() {
                let keyCode = event.keyCode 
                // Tab, Enter, space, Comma
                if(keyCode == 9 || keyCode == 13 || keyCode == 32 || keyCode == 188) {
                    event.preventDefault();
                    let validMsg = this.validTag(this.newTag)

                    if(validMsg != '') {
                        alert(validMsg)
                        return false
                    }

                    this.tags.push(this.newTag)
                    this.tagChange()
                    this.newTag = ''
                }
                // BackSpace
                else if(keyCode == 8) {
                    if(this.newTag == '') {
                        event.preventDefault();
                        this.tags.pop()
                        this.tagChange()
                    }
                }
            },

            validTag(tag) {
                let regx = /^[ㄱ-힣|\d|a-z|A-Z]+$/
                let msg = ''

                if(!regx.test(tag))
                    msg = '특수 문자는 포함할 수 없습니다.'
                else if(this.tags.indexOf(tag) != -1)
                    msg = '이미 등록한 태그 입니다.'
                
                return msg
            }
        }

    }
</script>

<style scoped>
.vue-input-tag-wrapper {
    border: 1px solid #ced4da;
    padding: 0.375rem 0.75rem calc(0.375rem - 4px);
    display: flex;
    flex-wrap: wrap;
}

.vue-input-tag-wrapper .input-tag {
    display: inline-block;
    font-size: 0.8rem;
    margin-bottom: 4px;
    margin-right: 4px;
    padding: 3px;
    cursor: default;
    border: 1px solid;
    border-radius: 2px;

    background-color: #cde69c;
    border-color: #a5d24a;
    color: #638421;
}

.vue-input-tag-wrapper .input-tag .remove {
    cursor: pointer;
    font-weight: bold;
}

/* .vue-input-tag-wrapper .input-tag .remove::before {
    content: "x";
} */

.vue-input-tag-wrapper .new-tag {
  background: transparent;
  border: 0;
  outline: none;
  flex-grow: 1;
}
</style>