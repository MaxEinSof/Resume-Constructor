<template>
    <div class="form-control">
        <label
            v-if="label"
            :for="id"
        > {{ label }} </label>
        <input
            :type="type"
            :id="id"
            :placeholder="placeholder"
            :value="modelValue"
            @input="change"
        />
    </div>
</template>

<script>
export default {
    props: {
        type: {
            type: String,
            required: false,
            default: 'text',
        },
        id: {
            type: String,
            required: true,
        },
        label: {
            type: String,
            required: true,
        },
        placeholder: {
            type: String,
            required: false,
        },
        modelValue: {
            type: String,
            required: false,
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
