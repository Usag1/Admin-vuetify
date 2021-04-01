<template>
    <nav>
        <v-toolbar dark app>
            <v-app-bar-nav-icon @click="drawer = !drawer" />
            <v-toolbar-title class="text-uppercase teal--text">
                Karin's<span class="font-weight-light">Mind</span> Admin
            </v-toolbar-title>
            <v-spacer></v-spacer>
            <v-toolbar-items>
                <v-btn @click="newPostDialog = !newPostDialog" flat>New Post</v-btn>
            </v-toolbar-items>
        </v-toolbar>

        <v-dialog v-model="newPostDialog" persistent max-width="768">
            <v-card>
                <v-card-title class="grey darken-4 white--text headline">
                    New Post
                </v-card-title>
                <v-card-text>
                    <v-container fluid grid-list-xl>
                        <v-layout wrap>
                            <v-flex xs12 md6>
                                <v-text-field label="Title" v-model="newPost.title" />
                            </v-flex>
                            <v-flex xs12 md6>
                                <v-text-field label="Author" v-model="newPost.author" />
                            </v-flex>
                            <v-flex xs12>
                                <v-textarea label="Content" v-model="newPost.content" />
                            </v-flex>
                        </v-layout>
                    </v-container>
                </v-card-text>
                <v-card-actions class="grey darken-4">
                    <v-btn @click="newPostDialog = !newPostDialog" flat color="grey darken-4 grey--text">Close</v-btn>
                    <v-spacer></v-spacer>
                    <v-btn @click="addPost" flat color="grey darken-4 teal--text">Add Post</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>

        <v-navgation-drawer v-model="drawer" class="grey darken-4" app clipped>
            <h2 class="headline white--text">Hello, World!</h2>
        </v-navgation-drawer>
    </nav>
</template>

<script>
    export default {
        name: 'Navbar',
        data () {
            return {
                drawer: false,
                newPostDialog: false,
                newPost: {}
            }
        },
        methods: {
            addPost () {
                let payload = {
                    title: this.newPost.title,
                    author: this.newPost.author,
                    content: this.newPost.content
                }

                fetch("https://localhost:3000/api/post/new", {
                    method: "POST",
                    headers: {
                        "content-type": "application/json"
                    },
                    body: JSON.stringify(payload)
                })
                .then(data => {
                    return data.json()
                })
                .then(json => {
                    this.$emit('newPost',json.result);
                    this.newPostDialog = false;
                    this.newPost = {};
                });
            }
        }
    }
</script>