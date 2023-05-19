<template>
    <div class="switch">
        <div class="demo">
            <div class="demo__left">
                <my-multiselect
                    :options="values"
                    display-property="title"
                    value-property="shortcut"
                    v-model="selectedValue"
                    @imput="handleSelectedValueChange"
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
                // { title: "bar", shortcut: "value1" },
                // { title: "some", shortcut: "value2" },
                // { title: "test", shortcut: "value4" },
                // { title: "bee", shortcut: "value5" },
                // { title: "look", shortcut: "value6" }
                1, 2, 3
            ],
            selectedValue: [1, 2, 3],
        };
    },
    components: {
        MyMultiselect,
    },
    methods: {
        handleSelectedValueChange() {
            this.selectedValue = this.selectedValue.map((value) => {
                if (typeof value === "number") {
                    // Перетворюємо примітивне число на об'єкт
                    const option = this.values.find((opt) => opt.shortcut === value);
                    return option ? { ...option } : null;
                }
                return value;
            });
        },
    },
};
</script>
