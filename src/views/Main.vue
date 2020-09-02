<template>
  <div class="main">

    <b-field label="New Note" type="is-danger" message="Enter a new note here">
      <b-input type="text" v-model="message" maxlength="30"> </b-input>
    </b-field>
    <b-button type="is-danger" @click="newNote">New Note</b-button><br /><br />

        <b-field label="New Note" type="is-danger" message="Edit a Note Here">
      <b-input type="text" v-model="editmessage" maxlength="30"> </b-input>
    </b-field>
    <b-button type="is-danger" @click="editNote" v-bind:id="editid">Edit Note</b-button><br /><br />
    <ul>
      <li v-for="note of notes" v-bind:key="note.id">
        <b-message title="Default" aria-close-label="Close message">
          {{ note.message }}
          <button
            v-bind:id="note.id"
            class="button is-success is-outlined"
            @click="deleteNote"
          >
            Delete
          </button>
          <button
            v-bind:id="note.id"
            class="button is-success is-outlined"
            @click="() => {editSelect(note.id, note.message)}"
          >
            edit
          </button>
        </b-message>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Main",
  data: function() {
    return {
      notes: [],
      message: "",
      editmessage: "",
      editid: null
    };
  },
  created: function() {
    this.getNotes();
  },
  methods: {
    newNote: function() {
      const { tokens, URL } = this.$route.query;

      fetch(`${URL}/notes/`, {
        method: "post",
        headers: {
          authorization: `Bearer ${tokens.access}`,
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ message: this.message }),
      }).then(() => {
        this.getNotes();
      });
    },
    getNotes: function() {
      const { tokens, URL } = this.$route.query;

      fetch(`${URL}/notes/`, {
        method: "get",
        headers: {
          authorization: `Bearer ${tokens.access}`,
        },
      })
        .then((response) => response.json())
        .then((data) => {
          this.notes = data;
        });
    },
    deleteNote: function(event) {
      const { tokens, URL } = this.$route.query;
      const id = event.target.id;

      fetch(`${URL}/notes/${id}/`, {
        method: "delete",
        headers: {
          authorization: `Bearer ${tokens.access}`,
        },
      }).then(() => {
        this.getNotes();
      });
    },
    editSelect: function(id, content){
      this.editid = id
      this.editmessage = content
    },
    editNote: function() {
      const { tokens, URL } = this.$route.query;
      const id = this.editid;

      fetch(`${URL}/notes/${id}/`, {
        method: "put",
        headers: {
          authorization: `Bearer ${tokens.access}`,
          "Content-Type": "application/json"
        },
        body: JSON.stringify({message: this.editmessage})
      }).then(() => {
        this.getNotes();
      });
    }
  },
};
</script>

<style>
div.main {
  width: 70%;
  min-width: 300px;
  margin: 10px auto;
}
</style>
