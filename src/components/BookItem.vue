<template>
    <li class="book">
        <div class="container">
            <img v-bind:src="book.imageUrl" />
            <div class="description">
                <div class="title">{{ book.title }}</div>
                <div class="author">{{ book.authorName }}</div>
            </div>
        </div>
        <hr/>
    </li>
</template>
<script>
export default {
    props: {
        book:{
            type: Object,
            required: true
        }
    }
}
</script>
<style scoped>
    li.book {
        display:block;
        padding: 6px 0 0 0;
    }
    div.container{
        position: relative;
        margin-left:-90%
    }
    div.description {
        display: inline-block;
        position:absolute; top:50%;
        transform: translate(0, -50%);
        width:40%
    }
    div.title {
        padding: 0px 24px;
        font-weight:bold;
    }
    div.author {
        padding: 6px 24px;
    }
    hr {
        border:0;
        height:1px;
        background: #35495E;
        background-image: linear-gradient(to right, #FFF, #42b983, #FFF)
    }

</style>