<template>
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
</template>

<script>
import AppButton from "@/components/AppButton";
import AppLoader from "@/components/AppLoader";
import AppComment from "@/components/AppComment";

export default {
    data() {
        return {
            comments: [],
            loading: false,
        }
    },
    computed: {
        isButtonVisible() {
            return !this.comments.length && !this.loading;
        },
    },
    emits: ['show-alert'],
    methods: {
        async loadComments() {
            this.loading = true;

            try {
                const response = await fetch('https://resume-constructor-99816-default-rtdb.firebaseio.com/comments.json');
                const result = await response.json();

                if (!result) {
                    throw new Error('Список комментариев пуст');
                }

                this.comments = result;

                this.$emit('show-alert', {
                    type: 'primary',
                    title: 'Успех',
                    text: this.getSuccessMessage(),
                });

            } catch (e) {
                this.$emit('show-alert', {
                    type: 'danger',
                    title: 'Ошибка',
                    text: e.message,
                });
            }

            this.loading = false;
        },
        getSuccessMessage() {
            const number = this.comments.length;
            const declensions = ['комментарий', 'комментария', 'комментариев'];
            const cases = [2, 0, 1, 1, 1, 2];
            return `Загружено ${number} ${declensions[(number % 100 > 4 && number % 100 < 20) ? 2 : cases[(number % 10 < 5) ? number % 10 : 5]]}`;
        },
    },
    components: {AppButton, AppLoader, AppComment},
}
</script>
