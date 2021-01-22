<template>
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

        <div v-if="infoBlocks.length">
            <app-info-block
                v-for="item in infoBlocks"
                :key="item.id"
                :id="item.id"
                :infoBlocks="infoBlocks"
                v-model:subtitleValue="item.subtitle"
                v-model:textValue="item.text"
                @update-info-blocks="updateInfoBlocks"
                @show-alert="showAlert"
            ></app-info-block>
        </div>
    </div>
</template>

<script>
import AppTitle from "@/components/AppTitle";
import AppAvatar from "@/components/AppAvatar";
import AppInfoBlock from "@/components/AppInfoBlock";

export default {
    computed: {
        isEmpty() {
            return !this.title && !this.avatarUrl && !this.infoBlocks.length;
        },
    },
    props: ['title', 'avatarUrl', 'infoBlocks'],
    emits: ['update-info-blocks', 'show-alert'],
    methods: {
        updateInfoBlocks(infoBlocks) {
            this.$emit('update-info-blocks', infoBlocks);
        },
        showAlert(alert) {
            this.$emit('show-alert', alert);
        },
    },
    components: {AppTitle, AppAvatar, AppInfoBlock},
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
