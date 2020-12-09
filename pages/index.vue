<template>
  <div id="content" class="has-side-bar">
    <div v-if="dataReady" id="side-bar">
      <ul id="note-nav">
        <li
          v-for="(note, index) in notes"
          :key="index"
          :ref="'note-li-' + index"
          :class="isActive(index)"
        >
          <div
            v-if="note.title !== ''"
            class="inner"
            @click="selectNote(index)"
          >
            <i class="las la-edit"></i>{{ note.title }}
          </div>
          <div v-else class="inner" @click="selectNote(index)">
            <i class="las la-edit"></i>Untitled
          </div>
          <div>
            <button class="delete-note" @click="deleteNote(index)">
              <i class="lar la-trash-alt"></i>
            </button>
          </div>
        </li>
      </ul>
      <button class="add-note" @click="addNote()">
        <i class="las la-plus-circle"></i>Add Note
      </button>
    </div>
    <div v-if="dataReady" id="inner">
      <Note v-if="notes && currentNote" :note.sync="currentNote" />
      <p v-else class="no-notes">Press "Add Note" to add your first note.</p>
    </div>
    <div v-else class="loading">Loading Data...</div>
  </div>
</template>

<script>
import Note from "~/components/Note";

export default {
  name: "Notes",

  components: {
    Note
  },

  data() {
    return {
      noteSelected: 0,
      notes: [],
      dataReady: false
    };
  },

  computed: {
    currentNote() {
      return this.notes[this.noteSelected];
    }
  },

  watch: {
    currentNote: {
      handler(val) {
        this.saveNotes();
      },
      deep: true
    }
  },

  async mounted() {
    const notes = await this.$localForage.getItem("Notes");
    if (notes) {
      this.notes = notes;
    }
    this.dataReady = true;
  },

  methods: {
    async saveNotes() {
      await this.$localForage.setItem("Notes", this.notes);
      console.log("Notes saved");
    },
    addNote() {
      this.notes.push({ title: "", content: "" });
      this.noteSelected = this.notes.length - 1;
      this.saveNotes();
    },
    deleteNote(index) {
      if (index > -1) {
        if (this.noteSelected !== 0) {
          this.noteSelected--;
        }
        this.notes.splice(index, 1);
      }
      this.saveNotes();
    },
    selectNote(index) {
      this.noteSelected = index;
    },
    isActive(index) {
      if (index === this.noteSelected) {
        return "active";
      }
    }
  }
};
</script>
