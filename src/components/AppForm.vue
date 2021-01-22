<template>
    <form
        class="card card-w30"
        @submit.prevent="addInfo"
    >
        <app-select
            id="type"
            label="Тип блока"
            v-model="typeInputValue"
            :options="selectOptions"
        ></app-select>

        <app-input
            v-if="isInfoBlockSelected"
            type="text"
            id="subtitle"
            label="Подзаголовок"
            placeholder="Введите подзаголовок"
            v-model="subtitleInputValue"
        ></app-input>

        <app-textarea
            rows="3"
            id="text"
            label="Значение"
            :placeholder="textInputPlaceholder"
            v-model="textInputValue"
        ></app-textarea>

        <app-button
            type="submit"
            :isDisabled="isButtonDisabled"
        >Добавить</app-button>
    </form>
</template>

<script>
import AppSelect from "@/components/AppSelect";
import AppInput from "@/components/AppInput";
import AppTextarea from "@/components/AppTextarea";
import AppButton from "@/components/AppButton";

export default {
    data() {
        return {
            selectOptions: [{
                value: 'title',
                text: 'Заголовок',
            },{
                value: 'avatar',
                text: 'Аватар',
            },{
                value: 'info-block',
                text: 'Блок информации',
            }],
            typeInputValue: 'title',
            subtitleInputValue: '',
            textInputValue: '',
        }
    },
    computed: {
        textInputPlaceholder() {
            switch (this.typeInputValue) {
                case 'title':
                    return 'Введите заголовок';
                case 'avatar':
                    return 'Введите url изображения';
                case 'info-block':
                    return 'Введите текст';
            }
        },
        isInfoBlockSelected() {
            return this.typeInputValue === 'info-block';
        },
        isButtonDisabled() {
            if (this.isInfoBlockSelected) {
                return this.textInputValue.length <= 3 || this.subtitleInputValue.length <= 3;
            }

            return this.textInputValue.length <= 3;
        },
    },
    props: ['infoBlocks'],
    emits: ['update-title', 'update-avatar-url', 'update-info-blocks'],
    methods: {
        addInfo() {
            switch (this.typeInputValue) {
                case 'title':
                    this.$emit('update-title', this.textInputValue);
                    this.uploadInfo('title', this.textInputValue);
                    break;
                case 'avatar':
                    this.$emit('update-avatar-url', this.textInputValue);
                    this.uploadInfo('avatarUrl', this.textInputValue);
                    break;
                case 'info-block':
                    const infoBlock = {
                        subtitle: this.subtitleInputValue,
                        text: this.textInputValue,
                    };
                    this.uploadInfoBlock(infoBlock);
                    break;
            }

            this.resetForm();
        },
        async uploadInfo(prop, value) {
            await fetch(`https://resume-constructor-99816-default-rtdb.firebaseio.com/resumeData/${prop}.json`, {
                method: 'PUT',
                body: JSON.stringify(value),
            });
        },
        async uploadInfoBlock(infoBlock) {
            const response = await fetch(`https://resume-constructor-99816-default-rtdb.firebaseio.com/resumeData/infoBlocks.json`, {
                method: 'POST',
                body: JSON.stringify(infoBlock),
            });

            const result = await response.json();

            const infoBlocks = this.infoBlocks.concat([{
                id: result.name,
                ...infoBlock
            }]);

            this.$emit('update-info-blocks', infoBlocks);
        },
        resetForm() {
            this.typeInputValue = 'title';
            this.subtitleInputValue = '';
            this.textInputValue = '';
        },
    },
    components: {AppSelect, AppInput, AppTextarea, AppButton},
}
</script>
