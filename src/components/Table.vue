<template>
  <div v-if="showForm" class="overlay">
    <TaskDialog
      @submit-form="submitForm"
      @cancel-form="showForm = false"
      action="add"
      :cache="entries"
    />
  </div>
  <div v-if="showEditForm" class="overlay">
    <TaskDialog
      @edit-form="editForm"
      @cancel-form="showEditForm = false"
      action="edit"
      :cache="entries"
      :editId="editId"
    />
  </div>
  <div class="container-md">
    <!-- Table -->
    <div class="panel panel-basic">
      <!-- Table header -->
      <div class="panel-header bg-primary">
        <div class="container">
          <div class="row d-flex justify-content-end">
            <div class="col-xs-4 col-xs-offset-3">
              <span class="glyphicon glyphicon-menu-hamburger"></span>
              <span> FRAMEWORKS </span>
            </div>
            <div class="col-xs-3 col-xs-offset-2">
              <button
                type="button"
                name="add"
                class="btn btn-primary"
                @click="showForm = true"
              >
                <span class="glyphicon glyphicon-plus-sign"></span
                ><span> ADD </span>
              </button>
            </div>
          </div>
        </div>
      </div>
      <!-- Table body -->
      <table class="panel-body table table-striped">
        <thead>
          <tr>
            <th class="text-center">Title</th>
            <th class="text-center">Description</th>
            <th class="text-center">Deadline</th>
            <th class="text-center">Priority</th>
            <th class="text-center">is Complete</th>
            <th class="text-center">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in entries" :key="index">
            <td>{{ item.title }}</td>
            <td>{{ item.description }}</td>
            <td>{{ item.deadline }}</td>
            <td>{{ item.priority }}</td>
            <td>
              <div class="form-check">
                <input
                  type="checkbox"
                  class="form-check-input"
                  value=""
                  :id="index"
                  v-model="item.isComplete"
                />
              </div>
            </td>
            <td>
              <button
                type="button"
                class="btn btn-primary"
                :class="{ invisible: item.isComplete }"
                @click="
                  showEditForm = true;
                  editId = index;
                "
              >
                UPDATE</button
              ><br />
              <button
                type="button"
                class="btn btn-danger"
                @click="deleteEntry(index)"
              >
                DELETE
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import TaskDialog from './TaskDialog.vue';
import toastr from 'toastr';
import 'toastr/build/toastr.css';

export default {
  name: 'Table',
  components: {
    TaskDialog,
  },
  data() {
    return {
      entries: [],
      showForm: false,
      showEditForm: false,
      editId: -1,
      numEntries: 0,
    };
  },
  methods: {
    submitForm(p_title, p_description, p_deadline, p_priority) {
      // push entry
      this.entries.push({
        title: p_title,
        description: p_description,
        deadline: p_deadline,
        priority: p_priority,
        isComplete: false,
      });

      this.numEntries++;
      this.showForm = false;
    },
    editForm(p_title, p_description, p_deadline, p_priority) {
      this.entries[this.editId] = {
        title: p_title,
        description: p_description,
        deadline: p_deadline,
        priority: p_priority,
        isComplete: false,
      };

      this.showEditForm = false;
    },
    deleteEntry(index) {
      this.entries.splice(index, 1);
      this.numEntries--;

      toastr['success']('Deleted!', '', {
        closeButton: true,
        timeOut: 3000,
        positionClass: 'toast-bottom-right',
      });
    },
    updateUpdateButton(idNumber) {
      this.entries[idNumber].isComplete;
    },
  },
};
</script>

<style scoped>
.container {
  position: relative;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}

.panel-header > .container {
  padding: 10px;
}

.invisible {
  visibility: hidden;
}
</style>
