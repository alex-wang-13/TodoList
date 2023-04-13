<template>
  <!-- Form header -->
  <div class="panel panel-primary">
    <div class="panel-header bg-primary">
      <div class="container">
        <div class="row justify-content-start">
          <span
            class="glyphicon glyphicon-plus-sign"
            v-if="submit === 'ADD'"
          ></span>
          <span
            class="glyphicon glyphicon-edit"
            v-if="submit === 'EDIT'"
          ></span>
          <span> {{ ' ' + heading + ' task ' }} </span>
        </div>
      </div>
    </div>
    <!-- Form body -->
    <div class="panel-body">
      <div class="container">
        <!-- Form fields -->
        <!-- Text fields -->
        <div class="row" v-if="submit === 'ADD'">
          <div class="col">
            <label for="title" class="form-label labels">Title:</label>
            <input
              class="form-control"
              :class="{ 'red-input': invalidTitle || repeatedTitle }"
              type="text"
              placeholder="Title"
              id="title"
              v-model="title"
            />
            <div class="red-text pull-left" v-if="invalidTitle">
              Enter a title.
            </div>
            <div class="red-text pull-left" v-if="repeatedTitle">
              This title already exists.
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col">
            <label for="description" class="form-label labels"
              >Description:</label
            >
            <input
              class="form-control"
              :class="{ 'red-input': invalidDescription }"
              type="text"
              placeholder="Description"
              id="description"
              v-model="description"
            />
            <div class="red-text pull-left" v-if="invalidDescription">
              Enter a description.
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col">
            <label for="deadline" class="form-label labels">Deadline:</label>
            <input
              class="form-control"
              :class="{ 'red-input': invalidDeadline }"
              type="date"
              id="deadline"
              v-model="deadline"
            />
            <div class="red-text pull-left" v-if="invalidDeadline">
              Enter a deadline.
            </div>
          </div>
        </div>
        <!-- Radios -->
        <div class="row">
          <div class="col">
            <div class="container">
              <div class="row">
                <label for="radios" class="form-label labels">Priority:</label>
              </div>
              <div class="form-check" id="radios">
                <div class="row justify-content-start">
                  <div class="col-xs-3">
                    <input
                      class="form-check-input"
                      :class="{ 'red-input': invalidPriority }"
                      name="priority"
                      type="radio"
                      id="low-radio"
                      @click="priority = 'Low'"
                      ref="low"
                    />
                    <label class="form-check-label" for="low-radio">
                      &nbspLow
                    </label>
                  </div>
                  <div class="col-xs-3">
                    <input
                      class="form-check-input"
                      :class="{ 'red-input': invalidPriority }"
                      name="priority"
                      type="radio"
                      id="med-radio"
                      @click="priority = 'Med'"
                      ref="med"
                    />
                    <label class="form-check-label" for="med-radio">
                      &nbspMed
                    </label>
                  </div>
                  <div class="col-xs-3">
                    <input
                      class="form-check-input"
                      :class="{ 'red-input': invalidPriority }"
                      name="priority"
                      type="radio"
                      id="high-radio"
                      @click="priority = 'High'"
                      ref="high"
                    />
                    <label class="form-check-label" for="high-radio">
                      &nbspHigh
                    </label>
                  </div>
                </div>
              </div>
            </div>
            <div class="red-text pull-left" v-if="invalidPriority">
              Select a priority.
            </div>
          </div>
        </div>
        <br />
        <!-- Cancel -->
        <footer>
          <button
            type="button"
            class="btn btn-danger"
            style="float: right"
            @click="cancelForm"
          >
            CANCEL
          </button>
          <!-- Submit -->
          <button
            type="button"
            class="btn btn-primary"
            style="float: right"
            @click="submitForm"
          >
            <span
              class="glyphicon glyphicon-plus-sign"
              v-if="submit === 'ADD'"
            ></span>
            <span
              class="glyphicon glyphicon-edit"
              v-if="submit === 'EDIT'"
            ></span>
            {{ submit }}
          </button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>
import toastr from 'toastr';
import 'toastr/build/toastr.css';

export default {
  name: 'TaskDialog',
  data() {
    return {
      localEntries: [],
      title: '',
      description: '',
      deadline: '',
      priority: '',
      heading: '',
      submit: '',
      invalidTitle: false,
      invalidDescription: false,
      invalidDeadline: false,
      invalidPriority: false,
      repeatedTitle: false,
    };
  },
  methods: {
    submitForm() {
      this.invalidTitle = !this.title.trim();
      this.invalidDescription = !this.description.trim();
      this.invalidDeadline = !this.deadline.trim();
      this.invalidPriority = !this.priority;

      // check for repeated title
      let titles = this.localEntries.map((entry) => entry.title);
      this.repeatedTitle = titles.includes(this.title);

      if (
        this.invalidTitle ||
        this.invalidDescription ||
        this.invalidDeadline ||
        this.invalidPriority
      ) {
        return;
      }

      if (this.repeatedTitle) {
        return;
      }

      let message = '';
      if (this.submit === 'ADD') {
        message = 'submit-form';
        toastr['success']('Added!', '', {
          closeButton: true,
          timeOut: 3000,
          positionClass: 'toast-bottom-right',
        });
      }
      if (this.submit === 'EDIT') {
        message = 'edit-form';
        toastr['success']('Edited!', '', {
          closeButton: true,
          timeOut: 3000,
          positionClass: 'toast-bottom-right',
        });
      }
      this.$emit(
        message,
        this.title,
        this.description,
        this.deadline,
        this.priority
      );
    },
    cancelForm() {
      this.$emit('cancel-form');
    },
    isValidDate(dateString) {
      const dateRegex = /^\d{4}-\d{2}-\d{2}$/;
      if (!dateRegex.test(dateString)) {
        return false; // Invalid format (YYYY-MM-DD)
      }
      const dateObj = new Date(dateString);
      if (isNaN(dateObj.getTime())) {
        return false; // Invalid date
      }
      return true; // Valid date
    },
  },
  props: {
    action: {
      type: String,
      required: true,
    },
    cache: {
      type: Array,
    },
    editId: {
      type: Number,
      default: undefined,
    },
  },
  mounted() {
    this.heading = this.action.charAt(0).toUpperCase() + this.action.slice(1);
    this.submit = this.action.toUpperCase();
    this.localEntries = this.cache == null ? [] : this.cache;
    // prepopulate fields for editing
    if (this.editId != null) {
      this.title = this.localEntries[this.editId].title;
      this.description = this.localEntries[this.editId].description;
      this.deadline = this.localEntries[this.editId].deadline;
      this.priority = this.localEntries[this.editId].priority;
      // remove this entry from localEntries
      this.localEntries[this.editId].title = '';
    }
    // prepopulating radios
    if (this.priority === 'Low') {
      this.$refs.low.checked = true;
    } else if (this.priority === 'Med') {
      this.$refs.med.checked = true;
    } else if (this.priority === 'High') {
      this.$refs.high.checked = true;
    }
  },
};
</script>

<style scoped>
.labels {
  float: left;
}

.panel-header > * {
  padding: 10px;
}

.red-input {
  border: 1px solid red;
}

.red-text {
  color: red;
  font-size: small;
}

#radios > * > * {
  padding: 0;
}
</style>
