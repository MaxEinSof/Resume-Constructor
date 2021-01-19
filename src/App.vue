<template>
    <div class="container column">
        <form
            class="card card-w30"
            @submit.prevent="addInfo"
        >
            <app-select
                id="type"
                label="Тип блока"
                v-model="typeInputValue"
                :options="[{
                    value: 'title',
                    text: 'Заголовок',
                },{
                    value: 'avatar',
                    text: 'Аватар',
                },{
                    value: 'info-block',
                    text: 'Блок информации',
                }]"
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

        <div class="card card-w70">
            <p v-if="isEmpty">Добавьте первый блок, чтобы увидеть результат</p>

            <app-title
                v-if="title"
                :title="title"
            ></app-title>

            <app-avatar
                v-if="avatarUrl"
                :avatarUrl="avatarUrl"
            ></app-avatar>

            <app-info-block
                v-if="infoBlocks.length"
                v-for="item in infoBlocks"
                :key="item.id"
                :id="item.id"
                v-model:subtitleValue="item.subtitle"
                v-model:textValue="item.text"
                @upload="uploadEditedInfoBlock"
                @delete="deleteInfoBlock"
            ></app-info-block>
        </div>
    </div>

    <div class="container">
        <p v-if="isButtonVisible">
            <app-button
                type="button"
                @action="loadComments"
            >Загрузить комментарии</app-button>
        </p>

        <app-loader v-if="loading"></app-loader>

        <div
            class="card"
            v-if="comments.length"
        >
            <h2>Комментарии</h2>
            <ul class="list">
                <app-comment
                    v-for="item in comments"
                    :key="item.id"
                    :email="item.email"
                    :body="item.body"
                ></app-comment>
            </ul>
        </div>
    </div>

    <app-alert
        v-if="alert"
        :alert="alert"
        @hide-alert="hideAlert"
    ></app-alert>
</template>

<script>
import AppSelect from "@/components/AppSelect";
import AppInput from "@/components/AppInput";
import AppTextarea from "@/components/AppTextarea";
import AppButton from "@/components/AppButton";
import AppTitle from "@/components/AppTitle";
import AppAvatar from "@/components/AppAvatar";
import AppInfoBlock from "@/components/AppInfoBlock";
import AppLoader from "@/components/AppLoader";
import AppComment from "@/components/AppComment";
import AppAlert from "@/components/AppAlert";

export default {
    data() {
        return {
            typeInputValue: 'title',
            subtitleInputValue: '',
            textInputValue: '',
            title: '',
            avatarUrl: '',
            infoBlocks: [],
            comments: [],
            loading: false,
            alert: null,
        }
    },
    mounted() {
        this.loadResumeData();
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
        isEmpty() {
            return !this.title && !this.avatarUrl && !this.infoBlocks.length;
        },
        isButtonVisible() {
            return !this.comments.length && !this.loading;
        },
    },
    methods: {
        async loadResumeData() {
            try {
                const response = await fetch('https://resume-constructor-99816-default-rtdb.firebaseio.com/resumeData.json');
                const {title, avatarUrl, infoBlocks} = await response.json();

                this.title = title;
                this.avatarUrl = avatarUrl;
                this.infoBlocks = Object.keys(infoBlocks).map(key => {
                    return {
                        id: key,
                        ...infoBlocks[key],
                    }
                });

            } catch (e) {
                console.log(e.message);
            }
        },
        addInfo() {
            switch (this.typeInputValue) {
                case 'title':
                    this.title = this.textInputValue;
                    this.uploadInfo('title');
                    break;
                case 'avatar':
                    this.avatarUrl = this.textInputValue;
                    this.uploadInfo('avatarUrl');
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
        async uploadInfo(value) {
            await fetch(`https://resume-constructor-99816-default-rtdb.firebaseio.com/resumeData/${value}.json`, {
                method: 'PUT',
                body: JSON.stringify(this[value]),
            });
        },
        async uploadInfoBlock(infoBlock) {
            const response = await fetch(`https://resume-constructor-99816-default-rtdb.firebaseio.com/resumeData/infoBlocks.json`, {
                method: 'POST',
                body: JSON.stringify(infoBlock),
            });

            const result = await response.json();

            this.infoBlocks.push({
                id: result.name,
                ...infoBlock
            });
        },
        resetForm() {
            this.typeInputValue = 'title';
            this.subtitleInputValue = '';
            this.textInputValue = '';
        },
        async uploadEditedInfoBlock(id, textarea) {
            try {
                textarea.style.height = '0px';
                textarea.style.height = textarea.style.height = `${textarea.scrollHeight + 5}px`;

                const {subtitle, text} = this.infoBlocks.find(block => block.id === id);

                await fetch(`https://resume-constructor-99816-default-rtdb.firebaseio.com/resumeData/infoBlocks/${id}.json`, {
                    method: 'PUT',
                    body: JSON.stringify({subtitle, text}),
                });

                this.alert = {
                    type: 'primary',
                    title: 'Успех',
                    text: `Блок "${subtitle}" был успешно отредактирован`,
                };

            } catch (e) {
                this.showAlert(e.message);
            }
        },
        async deleteInfoBlock(id) {
            try {
                const subtitle = this.infoBlocks.find(block => block.id === id).subtitle;

                await fetch(`https://resume-constructor-99816-default-rtdb.firebaseio.com/resumeData/infoBlocks/${id}.json`, {
                    method: 'DELETE',
                });

                this.infoBlocks = this.infoBlocks.filter(block => block.id !== id);

                this.alert = {
                    type: 'primary',
                    title: 'Успех',
                    text: `Блок "${subtitle}" был успешно удалён`,
                };

            } catch (e) {
                this.showAlert(e.message);
            }
        },
        async loadComments() {
            this.loading = true;

            try {
                const response = await fetch('https://resume-constructor-99816-default-rtdb.firebaseio.com/comments.json');
                const result = await response.json();

                if (!result) {
                    throw new Error('Список комментариев пуст');
                }

                this.comments = result;

                this.alert = {
                    type: 'primary',
                    title: 'Успех',
                    text: this.getSuccessMessage(),
                };

            } catch (e) {
                this.showAlert(e.message);
            }

            this.loading = false;
        },
        getSuccessMessage() {
            const number = this.comments.length;
            const declensions = ['комментарий', 'комментария', 'комментариев'];
            const cases = [2, 0, 1, 1, 1, 2];
            return `Загружено ${number} ${declensions[(number % 100 > 4 && number % 100 < 20) ? 2 : cases[(number % 10 < 5) ? number % 10 : 5]]}`;
        },
        showAlert(message) {
            this.alert = {
                type: 'danger',
                title: 'Ошибка',
                text: message,
            };
        },
        hideAlert() {
            setTimeout(() => {
                this.alert = null;
            }, 4000);
        },
    },
    components: {AppSelect, AppInput, AppTextarea, AppButton, AppTitle, AppAvatar, AppInfoBlock, AppLoader, AppComment, AppAlert},
}
</script>

<style lang="scss">
.avatar {
    display: flex;
    justify-content: center;

    img {
        width: 150px;
        height: auto;
        border-radius: 50%;
    }
}
</style>
