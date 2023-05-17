<template>
    <div class="switch">
        <div class="demo">
            <div class="demo__left">
                <my-multiselect
                    :options="values"
                    display-property="title"
                    value-property="shortcut"
                    v-model="selectedValue"

                ></my-multiselect>
            </div>
        </div>
    </div>
</template>

<script>
import MyMultiselect from "./components/MyMultiselect.vue";

export default {
    data() {
        return {
            values: [
                { title: "bar", shortcut: "value1" },
                { title: "boo", shortcut: "value2" },
                { title: "far", shortcut: "value3" },
                { title: "root", shortcut: "value4" },
                { title: "else", shortcut: "value5" },
                { title: "bar2", shortcut: "value6" },
                { title: "fizz2", shortcut: "value7" },
            ],
            selectedValue: ["value1", "value2", "value3", "value4"]
        };
    },
    components: {
        MyMultiselect,
    },
};
</script>
