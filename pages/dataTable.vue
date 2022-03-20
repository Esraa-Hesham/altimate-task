<template>
  <div class="data-table">
    <template>
      <div class="user-table">
        <div class="container-xl">
          <div class="table-responsive">
            <div class="table-wrapper">
              <div class="table-title">
                <div class="row">
                  <div class="col-sm-6">
                    <h2>Manage <b>Comments</b></h2>
                  </div>
                  <div class="col-sm-6">
                    <b-button @click="addCommentPopup()"
                      >Add New Comment</b-button
                    >

                    <downloadexcel
                      class="btn"
                      :fetch="fetchData"
                      :fields="comments"
                      :before-generate="startDownload"
                      :before-finish="finishDownload"
                    >
                      Download Excel
                    </downloadexcel>
                  </div>
                </div>
              </div>
              <table class="table table-striped table-hover">
                <thead>
                  <tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Body</th>
                    <th>Actions</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="comment in comments" :key="comment.id">
                    <td>{{ comment.id }}</td>
                    <td>{{ comment.name }}</td>
                    <td>{{ comment.email }}</td>
                    <td>{{ comment.body }}</td>
                    <td>
                      <b-button
                        class="btn btn-warning"
                        @click="editCommitPopup()"
                        >Edit</b-button
                      >
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>

        <!-- Add Modal HTML -->
        <b-modal id="my-modal" centered hide-footer>
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-body">
                <div class="form-group">
                  <label>Name</label>
                  <input
                    type="text"
                    class="form-control"
                    required
                    v-model="name"
                  />
                </div>

                <div class="form-group">
                  <label>Email</label>
                  <input
                    type="email"
                    class="form-control"
                    required
                    v-model="email"
                  />
                </div>
                <div class="form-group">
                  <label>body</label>
                  <input
                    type="text"
                    class="form-control"
                    required
                    v-model="body"
                  />
                </div>
                <input
                  type="button"
                  class="btn btn-default"
                  data-dismiss="modal"
                  value="Cancel"
                />
                <button class="btn btn-success" @click="addComment()">
                  Add
                </button>
              </div>
            </div>
          </div>
        </b-modal>
        <!-- Edit Modal HTML -->
        <b-modal id="my-modal-edit" centered hide-footer>
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-body">
                <div class="form-group">
                  <label>Name</label>
                  <input
                    type="text"
                    class="form-control"
                    required
                    v-model="commentToEdit.name"
                  />
                </div>

                <div class="form-group">
                  <label>Email</label>
                  <input
                    type="email"
                    class="form-control"
                    required
                    v-model="commentToEdit.email"
                  />
                </div>
                <div class="form-group">
                  <label>Body</label>
                  <input
                    type="text"
                    class="form-control"
                    required
                    v-model="commentToEdit.body"
                  />
                </div>
              </div>
              <div class="modal-footer">
                <input
                  type="button"
                  class="btn btn-default"
                  data-dismiss="modal"
                  value="Cancel"
                />
                <button
                  class="btn btn-info"
                  @click="edituser(commentToEdit.id)"
                >
                  Save
                </button>
              </div>
            </div>
          </div>
        </b-modal>
      </div>
    </template>
  </div>
</template>
<script>
export default {
  name: 'data-table',
  data() {
    return {
      name: '',
      email: '',
      body: '',
      commentToEdit: {},
      userToAdd: {},
      comments: [],
    }
  },
  async fetch() {
    this.comments = await fetch(
      'https://jsonplaceholder.typicode.com/comments'
    ).then((res) => res.json())
  },
  methods: {
    addCommentPopup() {
      this.$bvModal.show('my-modal')
    },
    addComment() {
      let lastId = this.comments.length
      var newRow = {
        id: lastId++,
        name: this.name,
        email: this.email,
        body: this.body,
      }
      this.comments.push(newRow)
      this.$bvModal.hide('my-modal')
    },
    editCommitPopup() {
      this.$bvModal.show('my-modal-edit')
    },

    edituser(commentId) {
      this.comments.forEach((comment) => {
        if (comment.id == commentId) {
          this.commentToEdit = Object.assign({}, comment)
        }
      }),
        this.$bvModal.hide('my-modal-edit')
    },
    startDownload() {
      alert('show loading')
    },
    finishDownload() {
      alert('hide loading')
    },
  },
}
</script>
