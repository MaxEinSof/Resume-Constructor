<template>
    <div class="form-control">
        <label :for="id"> {{ label }} </label>
        <select
            :id="id"
            :value="modelValue"
            @input="change"
        >
            <option
                v-for="item in options"
                :key="item"
                :value="item.value"
            >
                {{ item.text }}
            </option>
        </select>
    </div>
</template>

<script>
export default {
    props: {
        id: {
            type: String,
            required: true,
        },
        label: {
            type: String,
            required: true,
        },
        modelValue: {
            type: String,
            required: false,
        },
        options: {
            type: Array,
            required: true,
            validator(arr) {
                return arr[0] && 'value' in arr[0] && 'text' in arr[0];
            }
        },
    },
    emits: ['update:modelValue'],
    methods: {
        change(evt) {
            this.$emit('update:modelValue', evt.target.value);
        },
    },
}
</script>
