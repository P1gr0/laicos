<template>
    <div class="card comment">
        <div class="card-body py-2 row">
            <div class="col-2 col-lg-1">
                <img class="rounded-circle img-fluid"
                    :src="user.image ? `/images/profile/${user.image}` : `https://robohash.org/${user.name}.png?set=set4`"
                    data-holder-rendered="true">
            </div>
            <div class="col-9 col-lg-10">
                <h4 class="card-title"><a :href="`/users/${user.id}`">{{ user.name }}</a></h4>
                <p class="card-text fs-5" v-html="this.linkify(content)"></p>
                <img v-if="image" :src="`/images/${image}`" class="card-img text-center mb-2 w-50">
            </div>
            <div class="col-1">
                <AuthorMenu v-if="is_author" @update="updateComment" @remove="removeComment">Comment</AuthorMenu>
            </div>
        </div>
        <div class="card-footer bg-white d-flex justify-content-between">
            <like :baseUrl="'/comments'" :id="id" :likes="likes" :liked="liked"></like>
            <small>{{ getTimeFromNow(created_at) }}</small>
        </div>
    </div>
</template>

<script>
import * as utils from '../utils';
export default {
    props: ['image', 'content', 'is_author', 'id', 'created_at', 'user', 'likes', 'liked'],
    methods: {
        linkify(content) {
            return utils.linkify(content);
        },
        getTimeFromNow(creationDate) {
            return utils.timeSince(new Date(creationDate));
        },
        removeComment() {
            if (confirm('Comment will be removed permanently along with content. Are you sure?'))
                axios.delete("/comments/" + this.id).then(() => {
                    this.$emit("delete-comment", this.id);
                });
        },
        updateComment() {
            location.assign("/comments/" + this.id + "/edit");
        }
    }
}
</script>