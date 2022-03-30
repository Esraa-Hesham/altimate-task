<template>
  <div class="data-table my-5">
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
                    <b-button class="btn btn-create" @click="addCommentPopup()"
                      >Add New Comment</b-button
                    >

                    <v-layout>
                      <download-excel
                        class="btn btn-excel"
                        :data="comments"
                        :before-generate="startDownload"
                      >
                        Download Excel
                      </download-excel>
                    </v-layout>
                    <b-button class="btn btn-pdf" @click="prientPDF"
                      >Download PDf</b-button
                    >
                  </div>
                </div>
              </div>

              <div id="table_id">
                <table class="table table-striped table-hover">
                  <thead>
                    <tr>
                      <th>ID</th>
                      <th>Name</th>
                      <th>Email</th>
                      <th>Body</th>
                      <th id="actionsCul">Actions</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="comment in comments" :key="comment.id">
                      <td>{{ comment.id }}</td>
                      <td>{{ comment.name }}</td>
                      <td>{{ comment.email }}</td>
                      <td>{{ comment.body }}</td>
                      <td class="editBtn">
                        <b-button
                          class="btn btn-warning"
                          @click="editCommitPopup(comment.id)"
                          >Edit</b-button
                        >
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
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
      name: '',
    }
  },
  //fetch data from API
  async fetch() {
    this.comments = await fetch(
      'https://jsonplaceholder.typicode.com/comments'
    ).then((res) => res.json())
  },
  methods: {
    //PDF
    prientPDF() {
      let oldHtml = Object.assign(document.body.innerHTML, {})
      let actionsCul = document.getElementById('actionsCul')
      actionsCul.style.display = 'none'

      let btnEdit = document.querySelectorAll('.editBtn')
      for (let i = 0; i < btnEdit.length; i++) {
        btnEdit[i].style.display = 'none'
      }
      let printContents = document.getElementById('table_id').innerHTML
      document.body.innerHTML = printContents
      window.print()
      document.body.innerHTML = oldHtml
    },
    // Add User
    addCommentPopup() {
      this.$bvModal.show('my-modal')
    },
    addComment() {
      let lastId = this.comments.length + 1
      var newRow = {
        id: lastId++,
        name: this.name,
        email: this.email,
        body: this.body,
      }
      this.comments.push(newRow)
      this.name = ''
      this.email = ''
      this.body = ''
      this.$bvModal.hide('my-modal')
    },
    //Edit User
    editCommitPopup(commentId) {
      this.comments.forEach((comment) => {
        if (comment.id == commentId) {
          this.commentToEdit = Object.assign({}, comment)
        }
      }),
        this.$bvModal.show('my-modal-edit')
    },

    edituser(commentId) {
      this.comments = this.comments.map((comment) => {
        if (comment.id == commentId) {
          comment = this.commentToEdit
        }
        return comment
      })
      this.$bvModal.hide('my-modal-edit')
    },
    // alert to download Excel
    startDownload() {
      alert(' Loading Download Excel')
    },
  },
}
</script>
<style>
.btn {
  color: #eee;
  border-radius: 10px;
}
.btn-excel {
  background-color: rgb(10, 168, 63);
}
.btn-create {
  background-color: rgb(59, 117, 5);
}
.btn-pdf {
  background-color: rgb(136, 53, 15);
}
</style>
