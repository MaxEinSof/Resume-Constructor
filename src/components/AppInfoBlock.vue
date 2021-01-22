<template>
    <div class="info-block">
        <input
            type="text"
            :value="subtitleValue"
            :disabled="isDisabled"
            @input="updateSubtitleValue"
            v-focus
        >

        <textarea
            :value="textValue"
            :disabled="isDisabled"
            @input="updateTextValue"
            ref="textarea"
        ></textarea>

        <button
            class="edit-button"
            type="button"
            @click="toggleEditMode"
        >
            <svg v-if="isDisabled" xmlns="http://www.w3.org/2000/svg" width="20pt" height="20pt" viewBox="0 0 20 20">
                <path d="M 19.46875 3.246094 L 16.738281 0.515625 C 16.054688 -0.171875 14.859375 -0.171875 14.167969 0.515625 L 1.949219 12.855469 C 1.894531 12.910156 1.855469 12.976562 1.835938 13.050781 L 0.015625 19.421875 C -0.0273438 19.578125 0.015625 19.75 0.132812 19.867188 C 0.21875 19.953125 0.335938 20 0.453125 20 C 0.496094 20 0.539062 19.996094 0.578125 19.984375 L 6.941406 18.164062 C 7.015625 18.140625 7.082031 18.101562 7.136719 18.046875 L 19.46875 5.820312 C 19.8125 5.476562 20 5.019531 20 4.53125 C 20 4.046875 19.8125 3.589844 19.46875 3.246094 Z M 12.1875 3.824219 L 13.859375 5.496094 L 5.125 14.234375 L 4.496094 12.976562 C 4.421875 12.824219 4.261719 12.726562 4.089844 12.726562 L 3.363281 12.726562 Z M 1.117188 18.882812 L 1.707031 16.8125 L 3.1875 18.292969 Z M 6.363281 17.382812 L 4.1875 18.003906 L 1.996094 15.8125 L 2.617188 13.636719 L 3.808594 13.636719 L 4.59375 15.203125 C 4.636719 15.292969 4.707031 15.363281 4.796875 15.40625 L 6.363281 16.191406 Z M 7.273438 16.636719 L 7.273438 15.910156 C 7.273438 15.738281 7.175781 15.578125 7.023438 15.503906 L 5.765625 14.875 L 14.503906 6.140625 L 16.175781 7.8125 Z M 18.824219 5.1875 L 16.820312 7.171875 L 12.828125 3.179688 L 14.8125 1.175781 C 15.15625 0.832031 15.753906 0.832031 16.097656 1.175781 L 18.824219 3.902344 C 18.996094 4.074219 19.089844 4.300781 19.089844 4.546875 C 19.089844 4.789062 18.996094 5.015625 18.824219 5.1875 Z M 18.824219 5.1875 "/>
            </svg>

            <svg v-else xmlns="http://www.w3.org/2000/svg" width="20pt" height="20pt" viewBox="0 0 20 20">
                <path style=" stroke:none;fill-rule:nonzero;fill:rgb(0%,0%,0%);fill-opacity:1;" d="M 19.777344 2.640625 C 19.46875 2.320312 18.960938 2.3125 18.644531 2.621094 C 18.636719 2.625 18.632812 2.632812 18.625 2.640625 L 5.589844 15.671875 L 1.355469 11.4375 C 1.039062 11.132812 0.53125 11.140625 0.222656 11.460938 C -0.0742188 11.769531 -0.0742188 12.261719 0.222656 12.570312 L 5.023438 17.371094 C 5.335938 17.683594 5.84375 17.683594 6.15625 17.371094 L 19.753906 3.769531 C 20.074219 3.464844 20.082031 2.957031 19.777344 2.640625 Z M 19.777344 2.640625 "/>
            </svg>
        </button>

        <button
            class="delete-btn"
            type="button"
            @click="deleteInfoBlock"
        >
            <svg xmlns="http://www.w3.org/2000/svg" width="20pt" height="20pt" viewBox="0 0 20 20">
                <path d="M 12.753906 7.242188 C 12.492188 7.242188 12.285156 7.453125 12.285156 7.710938 L 12.285156 16.558594 C 12.285156 16.816406 12.492188 17.027344 12.753906 17.027344 C 13.011719 17.027344 13.222656 16.816406 13.222656 16.558594 L 13.222656 7.710938 C 13.222656 7.453125 13.011719 7.242188 12.753906 7.242188 Z M 12.753906 7.242188 "/>
                <path d="M 7.226562 7.242188 C 6.96875 7.242188 6.761719 7.453125 6.761719 7.710938 L 6.761719 16.558594 C 6.761719 16.816406 6.96875 17.027344 7.226562 17.027344 C 7.488281 17.027344 7.695312 16.816406 7.695312 16.558594 L 7.695312 7.710938 C 7.695312 7.453125 7.488281 7.242188 7.226562 7.242188 Z M 7.226562 7.242188 "/>
                <path d="M 3.203125 5.953125 L 3.203125 17.484375 C 3.203125 18.167969 3.453125 18.808594 3.890625 19.265625 C 4.324219 19.726562 4.929688 19.988281 5.5625 19.992188 L 14.417969 19.992188 C 15.054688 19.988281 15.65625 19.726562 16.09375 19.265625 C 16.527344 18.808594 16.777344 18.167969 16.777344 17.484375 L 16.777344 5.953125 C 17.648438 5.722656 18.210938 4.882812 18.09375 3.992188 C 17.976562 3.101562 17.21875 2.433594 16.320312 2.433594 L 13.921875 2.433594 L 13.921875 1.847656 C 13.925781 1.355469 13.730469 0.882812 13.382812 0.535156 C 13.035156 0.191406 12.5625 -0.00390625 12.070312 0 L 7.910156 0 C 7.417969 -0.00390625 6.945312 0.191406 6.597656 0.535156 C 6.25 0.882812 6.054688 1.355469 6.058594 1.847656 L 6.058594 2.433594 L 3.660156 2.433594 C 2.761719 2.433594 2.003906 3.101562 1.886719 3.992188 C 1.773438 4.882812 2.335938 5.722656 3.203125 5.953125 Z M 14.417969 19.054688 L 5.5625 19.054688 C 4.761719 19.054688 4.136719 18.367188 4.136719 17.484375 L 4.136719 5.992188 L 15.84375 5.992188 L 15.84375 17.484375 C 15.84375 18.367188 15.21875 19.054688 14.417969 19.054688 Z M 6.996094 1.847656 C 6.992188 1.605469 7.085938 1.371094 7.261719 1.199219 C 7.433594 1.027344 7.667969 0.933594 7.910156 0.9375 L 12.070312 0.9375 C 12.3125 0.933594 12.546875 1.027344 12.722656 1.199219 C 12.894531 1.371094 12.988281 1.605469 12.988281 1.847656 L 12.988281 2.433594 L 6.996094 2.433594 Z M 3.660156 3.371094 L 16.320312 3.371094 C 16.785156 3.371094 17.164062 3.75 17.164062 4.214844 C 17.164062 4.679688 16.785156 5.054688 16.320312 5.054688 L 3.660156 5.054688 C 3.195312 5.054688 2.816406 4.679688 2.816406 4.214844 C 2.816406 3.75 3.195312 3.371094 3.660156 3.371094 Z M 3.660156 3.371094 "/>
                <path d="M 9.992188 7.242188 C 9.730469 7.242188 9.523438 7.453125 9.523438 7.710938 L 9.523438 16.558594 C 9.523438 16.816406 9.730469 17.027344 9.992188 17.027344 C 10.25 17.027344 10.457031 16.816406 10.457031 16.558594 L 10.457031 7.710938 C 10.457031 7.453125 10.25 7.242188 9.992188 7.242188 Z M 9.992188 7.242188 "/>
            </svg>
        </button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            isDisabled: true,
        }
    },
    mounted() {
        this.setTextareaHeight();
    },
    directives: {
        focus: {
            updated(el) {
                el.focus();
            },
        }
    },
    props: {
        id: {
            type: String,
            required: true,
        },
        subtitleValue: {
            type: String,
            required: true,
        },
        textValue: {
            type: String,
            required: true,
        },
        infoBlocks: {
            type: Array,
            required: true,
        },
    },
    emits: {
        'update:subtitleValue'(value) {
            return !!value;
        },
        'update:textValue'(value) {
            return !!value;
        },
        'update-info-blocks': null,
        'show-alert': null,
    },
    methods: {
        setTextareaHeight() {
            this.$refs.textarea.style.height = '0px';
            this.$refs.textarea.style.height = `${this.$refs.textarea.scrollHeight + 5}px`;
        },
        updateSubtitleValue(evt) {
            this.$emit('update:subtitleValue', evt.target.value);
        },
        updateTextValue(evt) {
            this.$emit('update:textValue', evt.target.value);
        },
        toggleEditMode() {
            this.isDisabled = !this.isDisabled;

            if (this.isDisabled) {
                this.uploadEditedInfoBlock();
            }
        },
        async uploadEditedInfoBlock() {
            try {
                this.setTextareaHeight();

                const editedInfo = {
                    subtitle: this.subtitleValue,
                    text: this.textValue,
                };

                await fetch(`https://resume-constructor-99816-default-rtdb.firebaseio.com/resumeData/infoBlocks/${this.id}.json`, {
                    method: 'PUT',
                    body: JSON.stringify(editedInfo),
                });

                this.$emit('show-alert', {
                    type: 'primary',
                    title: 'Успех',
                    text: `Блок "${this.subtitleValue}" был успешно отредактирован`,
                });

            } catch (e) {
                this.$emit('show-alert', {
                    type: 'danger',
                    title: 'Ошибка',
                    text: e.message,
                });
            }
        },
        async deleteInfoBlock() {
            try {
                await fetch(`https://resume-constructor-99816-default-rtdb.firebaseio.com/resumeData/infoBlocks/${this.id}.json`, {
                    method: 'DELETE',
                });

                const infoBlocks = this.infoBlocks.filter(block => block.id !== this.id);
                this.$emit('update-info-blocks', infoBlocks);

                this.$emit('show-alert', {
                    type: 'primary',
                    title: 'Успех',
                    text: `Блок "${this.subtitleValue}" был успешно удалён`,
                });

            } catch (e) {
                this.$emit('show-alert', {
                    type: 'danger',
                    title: 'Ошибка',
                    text: e.message,
                });
            }
        },
    },
}
</script>

<style lang="scss">
.info-block {
    position: relative;

    input {
        display: block;
        width: 100%;
        margin: 22px 0;
        padding-right: 100px;
        font-family: Inter, Roboto, Oxygen, Fira Sans, Helvetica Neue, sans-serif;
        font-size: 1.65rem;
        font-weight: 500;
        line-height: 1.45;
        border: 1px solid #ccc;
        outline: none;

        &:disabled {
            border-color: transparent;
            border-bottom-color: #eaecef;
            background-color: transparent;
        }
    }

    textarea {
        display: block;
        width: 100%;
        min-height: 58px;
        margin: 16px 0;
        font-family: Inter, Roboto, Oxygen, Fira Sans, Helvetica Neue, sans-serif;
        font-size: 16px;
        line-height: 1.7;
        background-color: transparent;
        border: 1px solid #ccc;
        outline: none;
        resize: none;
        overflow: auto;

        &:disabled {
            border-color: transparent;
        }
    }
}

.delete-btn,
.edit-button {
    position: absolute;
    padding: 0;
    border: none;
    outline: none;
    background-color: transparent;
    cursor: pointer;
}

.delete-btn {
    top: 7px;
    right: 5px;
}

.edit-button {
    top: 8px;
    right: 45px;
}
</style>
