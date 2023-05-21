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
                333,
                {item: "2"},
                {item: "2", id: "222"},
                "222",
                "",
                [1]
                ],
            selectedValue: ["222"],
        };
    },
    components: {
        MyMultiselect,
    },
};
</script>
