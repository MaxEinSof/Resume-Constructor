<template>
    <div class="container column">
        <app-form
            :infoBlocks="infoBlocks"
            @update-title="updateTitle"
            @update-avatar-url="updateAvatarUrl"
            @update-info-blocks="updateInfoBlocks"
        ></app-form>

        <app-resume
            :title="title"
            :avatarUrl="avatarUrl"
            :infoBlocks="infoBlocks"
            @update-info-blocks="updateInfoBlocks"
            @show-alert="showAlert"
        ></app-resume>
    </div>

    <app-comments
        @show-alert="showAlert"
    ></app-comments>

    <app-alert
        v-if="alert"
        :alert="alert"
        @hide-alert="hideAlert"
    ></app-alert>
</template>

<script>
import AppForm from "@/components/AppForm";
import AppResume from "@/components/AppResume";
import AppComments from "@/components/AppComments";
import AppAlert from "@/components/AppAlert";

export default {
    data() {
        return {
            title: '',
            avatarUrl: '',
            infoBlocks: [],
            alert: null,
        }
    },
    mounted() {
        this.loadResumeData();
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
        updateTitle(title) {
            this.title = title;
        },
        updateAvatarUrl(avatarUrl) {
            this.avatarUrl = avatarUrl;
        },
        updateInfoBlocks(infoBlocks) {
            this.infoBlocks = infoBlocks;
        },
        showAlert(alert) {
            this.alert = alert;
        },
        hideAlert() {
            this.alert = null;
        },
    },
    components: {AppForm, AppResume, AppComments, AppAlert},
}
</script>
