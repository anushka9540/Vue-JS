<template>
    <div class="modal-box" :class="themeClass">
        <h2>{{ modalTitle }}</h2>
        <p>{{ modalContent }}</p>
    </div>
</template>

<script>
export default {
    name: 'ModalProject',
    props: {
        modalTitle: String,
        modalContent: String,
        theme: String
    },
    computed: {
        themeClass() {
            return this.theme === 'sales' ? 'sales-theme' : 'default-theme';
        }
    }
}
</script>

<style scoped>
.modal-box {
    height: 200px;
    width: 90%;
    max-width: 400px;
    border-radius: 10px;
    box-shadow: 8px 8px 10px rgba(0, 0, 0, 0.6);
    padding: 20px;
    text-align: center;
    transition: all 0.3s ease-in-out;
}


.default-theme {
    border: 2px solid;
    background-color: #4aa19f;
    color: black;
}


.sales-theme {
    border: 3px solid rgb(125, 196, 223);
    background-color: rgb(25, 85, 120);
    color: yellow;
}


p {
    position: relative;
    top: 50px;
    font-size: 1rem;
}


@media screen and (max-width: 600px) {
    .modal-box {
        width: 95%;
        height: auto;
        padding: 15px;
    }
    h2 {
        font-size: 1.2rem;
    }
    p {
        font-size: 0.9rem;
        top: 30px;
    }
}
</style>
