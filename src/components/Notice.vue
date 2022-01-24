<template>
    <div :class="{
        notice: true, 'shadow-box': true,
        primary: this.notice.type === 'primary',
        warning: this.notice.type === 'warning',
        danger: this.notice.type === 'danger',
        info: this.notice.type === 'info'
    }">
        <h4>{{ this.notice.title }}</h4>
        <p>{{ this.notice.content }}</p>
        <div class="flex flex-column time">
            Created at: {{ this.notice.createdAt }}
            <br>
            <span v-if="this.notice.updatedAt">Last updated: {{ this.notice.updatedAt }}</span>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Notice',
    props: {
        notice: {
            type: Object,
            required: true
        }
    },
}
</script>

<style scoped>
.notice {
    padding: 1.35rem;
}
.notice > h4 {
    margin: 0;
    font-size: calc(1rem + 0.3vw);
}
.time {
    font-size: calc(0.7rem + 0.1vw);
}
.time > span {
    margin-top: 0.2rem;
}
.primary {
    background-color: #5cdd8b;
}
.warning {
    background-color: #ffc107;
}
.danger {
    background-color: #ff5252;
}
.info {
    background-color: #5ae4ff;
}
</style>