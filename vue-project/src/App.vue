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
                { title: "foo", shortcut: "value1" },
                { title: "bar", shortcut: "value2" },
                { title: "fizz", shortcut: "value3" },
                { title: "buzz", shortcut: "value4" },
                { title: "foo2", shortcut: "value5" },
                { title: "bar2", shortcut: "value6" },
                { title: "fizz2", shortcut: "value7" }
            ],
            selectedValue: ["value1", "value2", "value3", "value4"]
        };
    },
    components: {
        MyMultiselect
    },

    methods: {}
};
</script>
