<template>
    <div class="background-card" tabindex="-1" ref="parent" @blur="focused = false">
        <h1>Chips</h1>
        <h4>Use chips prop to make selected option as chip.</h4>
        <div class="my-multiselect" :style="{ width: width }" @click="handleClick" tabindex="-1" ref="parent" @blur="focused = false">
            <div class="input-with-arrow">
        <span class="arrow" @click="rotateArrow">
          <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" aria-hidden="true" role="img" tag="i" class="v-icon notranslate v-theme--light v-icon--size-default iconify iconify--mdi" width="1em" height="1em" viewBox="0 0 24 24">
            <path fill="currentColor" d="m7 10l5 5l5-5H7Z"></path>
          </svg>
        </span>
            </div>

            <div class="my-multiselect__placeholder" v-if="modelValue.length === 0 || (showCount ? modelValue.length <= 4 : modelValue.length <= 12)">
                {{ getPlaceholderText(false) }}
            </div>
            <div class="my-multiselect__selected" v-for="(option, i) in formatedOptions" :key="i" v-show="option.checked && (showCount || formatedOptions.slice(i).filter(j => j.checked).length <= 4)">
                {{ option['value'] }}
                <span class="my-multiselect__remove" @click="preventClose($event); handleOptionClick(i)">&times;</span>
            </div>
            <div class="my-multiselect__options" v-show="focused" @click="preventClose" :style="{ top: optionsTop }">
                <div class="my-multiselect__option" :class="{ 'my-multiselect__option--checked': option.checked }" v-for="(option, i) in formatedOptions" :key="i" @click="handleOptionClick(i)">
                    <div class="my-multiselect__checkbox"></div>
                    <div class="my-multiselect__text">
                        {{ option['value'] }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    function renderItem(item, isRoot = true) {
        switch (typeof item) {
            case typeof {}:
                if (Array.isArray(item)) {
                    return isRoot ?
                        item.map(i => renderItem(i, false)).join(', ') :
                        `[ ${item.map(i => renderItem(i, false)).join(', ')} ]`;
                }
            const temp = Object.keys(item).map(i => `${i.toString()}: ${renderItem(item[i], false)}`).join(', ');
                return isRoot ? temp : `{ ${temp} }`;
            case typeof "":
                return isRoot ? item : `"${item}"`;
            case typeof null:
                return "null";
            case typeof 0:
                return item.toString();
            default:
                throw 1;
        }
    }
export default {
    data() {
        return {
            focused: false,
            optionsTop: "34px",
            showCount: false,
        };
    },
    props: {
        width: {
            type: String,
            default: "325px",
        },
        options: {
            type: Array,
            default: () => [],
        },
        modelValue: {
            type: Array,
            default: () => [],
        },
        placeholder: {
            type: String,
            default: "Chips",
        },
        displayProperty: {
            type: String,
            default: "name",
        },
        valueProperty: {
            type: String,
            default: "value",
        },
        required: {
            default: false,
        },
    },

    computed: {
        formatedOptions() {
            const _options = this.options.map(i => renderItem(i));
            return _options.map(option => {
                let checked = this.modelValue.some(item => renderItem(item) === option);
                return {
                    value: option,
                    checked
                };
            });
        }
    },

    mounted() {
        this.fixTop();
    },

    methods: {
        fixTop() {
            this.optionsTop = this.$refs.parent.clientHeight + -135 + "px";
        },

        rotateArrow() {
            const arrow = document.querySelector('.arrow');
            arrow.classList.toggle('rotated');
        },

        getPlaceholderText() {
            if (this.modelValue.length <= 4) {
                switch (typeof this.placeholder) {
                    case 'string':
                    case 'number':
                    case 'object':
                        if (Array.isArray(this.placeholder) || this.placeholder === null) {
                            return this.placeholder;
                        }
                    default:
                        return '';
                }
            } else {
                const remainingCount = this.modelValue.length - 4;
                switch (typeof this.placeholder) {
                    case 'string':
                    case 'number':
                    case 'object':
                        if (Array.isArray(this.placeholder) || this.placeholder === null) {
                            return `${this.placeholder} +${remainingCount}`;
                        }
                    default:
                        return `+${remainingCount}`;
                }
            }
        },

        handleOptionClick(i) {
            let newValue = [...this.modelValue];


            if (!this.formatedOptions[i].checked) {
                if (this.modelValue.length === 0 || !this.required) {
                    newValue.push(this.options[i]);
                }
            } else {
                let existIndex = this.modelValue.findIndex(
                    v => v === this.options[i]
                );
                newValue.splice(existIndex, 1);
            }
            this.$emit("update:modelValue", newValue);

            //   this.localOptions[i].checked = !this.localOptions[i].checked;
            console.log(this.$refs.parent);
            setTimeout(this.fixTop, 100);
        },
        preventClose(e) {
            e.stopPropagation();
        },
        handleClick() {
            this.focused = !this.focused;
        }
    }
};
</script>

<style scoped>
body{
    background-color: #f4f5fb;
}

.input-with-arrow {
    position: relative;
}

.input-with-arrow .arrow {
    position: absolute;
    top: 15px;
    left: 20px;
    transform: translateY(-50%);
    cursor: pointer;
    transition: transform 0.3s ease;
    color: #dcdbdd;
}

.input-with-arrow .arrow svg {
    fill: #aaa;
    width: 550px;
    height: 20px;
}

.input-with-arrow .arrow.rotated svg {
    transform: rotate(180deg);
    transition: transform 0.3s ease;
}


.background-card{
    padding: 10px 0px 40px 30px;
    width: 400px;
    left: 320px;
    z-index: -9999;
    background-color: #fff;
    border-radius: 6px;
    box-shadow: 0 4px 5px -2px;
}
.my-multiselect {
    padding: 6px 12px;
    margin: 8px 0;
    display: inline-block;
    border-radius: 4px;
    box-sizing: border-box;
    min-height: 33px;
    min-width: 222px;
    position: relative;
    display: flex;
    flex-wrap: wrap;
    outline: 1px solid #d2d1d3;
}

.my-multiselect:hover {
    outline: 1px solid #97969b;
    transition: .2s linear;
}

.my-multiselect:focus {
    outline: 2px solid #9156fd;
}

.my-multiselect__placeholder {
    color: #929292;
}

.my-multiselect__selected {
    padding: 4px 8px;
    padding-right: 0;
    margin: 3px 3px;
    border-radius: 99999px;
    background-color: #f3f3f4;
    color: #939198;
    z-index: 2;
}

.my-multiselect__remove {
    cursor: pointer;
    padding-right: 7px;
}

.my-multiselect__remove:hover {
    color: red;
    font-weight: bold;
}

.my-multiselect__options {
    position: absolute;
    right: 0;
    left: 0;
    display: flex;
    background: #fff;
    flex-direction: column;
    box-shadow: 0 0 3px 3px #ececed;
    padding: 5px 0;
    min-height: 55px;
    max-height: 188px;
    overflow-y: auto;
    border-radius: 3px;
}

.my-multiselect__option {
    padding: 6px 11px;
    cursor: pointer;
    display: flex;
    align-items: center;
}

.my-multiselect__option--checked {
    color: #504b56;
    font-weight: bold;
}

.my-multiselect__option--checked:hover {
    background-color: #eeeeef;
    font-weight: bold;
}

.my-multiselect__text:hover {
    background-color: #eeeeef;
    font-weight: bold;
}

.my-multiselect__text::after {
    background-color: #eeeeef;
    font-weight: bold;
}

.my-multiselect__option:hover{
    background-color: #eeeeef;
}

.my-multiselect__checkbox {
    width: 22px;
    height: 22px;
    border: 2px solid #969696;
    margin-right: 7px;
    position: relative;
    /* background: #1f7bb8; */
    border-radius: 3px;
}

.my-multiselect__option--checked .my-multiselect__checkbox {
    background: #504b56;
    border: none;
    border-radius: 3px;
}

.my-multiselect__option--checked .my-multiselect__checkbox::after {
    width: 14px;
    height: 9px;
    border-left: 2px solid rgb(255, 255, 255);
    border-bottom: 2px solid rgb(255, 255, 255);
    content: "";
    z-index: 9999;
    position: absolute;
    transform: rotate(-45deg);
    left: 4px;
    top: 4px;
}
</style>
