<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/>  -->
    <!-- <FullCalendar :options="calendarOptions" /> -->
    <FormPopup
      v-bind:dialogFormVisible="dialogFormVisible"
      :form="form"
      @form-popup-save="saveForm"
    />

    <FormUpdate
      v-if="formUpdate"
      :dialogFormVisible.sync="isShowUpdate"
      :form="formUpdate"
      @update="handleUpdate"
    />

    <div class="demo-app">
      <div class="demo-app-main">
        <FullCalendar
          ref="calendar"
          class="demo-app-calendar"
          :options="calendarOptions"
          >``
          <template v-slot:eventContent="arg">
            <b>{{ arg.timeText }}</b>
            <i>{{ arg.event.title }}</i>
          </template>
        </FullCalendar>
      </div>
    </div>

    <el-dialog :visible.sync="dialogVisible" width="30%">
      <span>Cập nhật hoặc xóa hoạt động</span>
      <span slot="footer" class="dialog-footer">
        <el-button
          type="primary"
          @click="
            dialogVisible = false;
            isShowUpdate = true;
          "
          >Cập nhật</el-button
        >
        <el-button
          @click="
            deleteEvent();
            changeDialogVisible();
          "
          >Xóa</el-button
        >

        <!-- <el-button v-on:click="handler('deleteEvent','dialogVisible = false')">Xóa</el-button>    -->
      </span>
    </el-dialog>
  </div>
</template>

<script>
import FullCalendar from "@fullcalendar/vue";
import dayGridPlugin from "@fullcalendar/daygrid";
import interactionPlugin from "@fullcalendar/interaction";
// import { INITIAL_EVENTS, createEventId } from './event-utils'

// import HelloWorld from './components/HelloWorld.vue'
import FormPopup from "./components/FormPopup";
import FormUpdate from "./components/FormUpdate";

export default {
  name: "App",
  components: {
    // HelloWorld,
    FormUpdate,
    FormPopup,
    FullCalendar, // make the <FullCalendar> tag available
  },
  data() {
    return {
      isShowUpdate: false,
      dialogVisible: false,
      dialogFormVisible: false,
      calendarOptions: {
        plugins: [
          dayGridPlugin,
          // timeGridPlugin,
          interactionPlugin, // needed for dateClick
        ],
        headerToolbar: {
          left: "prev,next today",
          center: "title",
          right: "dayGridMonth,timeGridWeek,timeGridDay",
        },
        initialView: "dayGridMonth",
        // initialEvents: INITIAL_EVENTS, // alternatively, use the `events` setting to fetch from a feed
        editable: true,
        selectable: true,
        selectMirror: true,
        dayMaxEvents: true,
        weekends: true,
        select: this.handleDateSelect,
        eventClick: this.handleEventClick,
        eventsSet: this.handleEvents,

        //events test
        events: [
          {
            id: "b",
            title: "The Title",
            start: "2020-12-01",
            end: "2020-12-02",
          },
        ],
      },
      // form popup

      form: {
        name: "",
        priority: "",
        start: null,
        end: null,
        desc: "",
      },
      formUpdate: null,
      currentEvents: [],
      // dialogVisible: false,
    };
  },
  methods: {
    changeDialogVisible() {
      this.dialogVisible = false;
    },

    handleUpdate() {
      this.formUpdate;
      const find = this.calendarOptions.events.find(
        (e) => e.id === this.formUpdate.id
      );
      if (find) {
        Object.assign(find, this.formUpdate);
        this.isShowUpdate = false;
      }
    },

    deleteEvent() {
      console.log("deleteEvent");
      const index = this.calendarOptions.events.findIndex((e) => e.id === this.formUpdate.id);
      if (index >= 0) {
        this.calendarOptions.events.splice(index, 1);
      }

      // console.log(this.calendarOptions.getEventById('a'));
      // // this.calendarOptions.getEventById('1') .remove();
    },

    saveForm(dataFormPopup) {
      let calendarApi = this.$refs.calendar.getApi(); // .view.calendar

      calendarApi.unselect(); // clear date selection
      this.calendarOptions.events.push({
        // id: ,
        title: dataFormPopup.name, // a property!
        start: dataFormPopup.start,
        end: dataFormPopup.end,
      });

      //post data to firebase

      // this.$http.post('https://fir-full-calendar-default-rtdb.firebaseio.com/posts', {
      //     name: this.form.name,
      //     priority:this.form.priority,
      //     date:this.form.date,
      //     time:this.form.time,
      //     desc:this.form.desc,
      //       }).then(function(data){
      //           console.log(data);
      //           // this.submitted = true;
      //       });
    },

    handleWeekendsToggle() {
      this.calendarOptions.weekends = !this.calendarOptions.weekends; // update a property
    },

    handleDateSelect(selectInfo) {
      this.dialogFormVisible = true;
      console.log(selectInfo);
      this.dialogFormVisible = "";
    },

    handleEventClick(data) {
      this.formUpdate = {
        id: data.event.id,
        title: data.event.title,
        name: data.event.name,
        priority: data.event.priority,
        start: data.event.start,
        end: data.event.end,
        desc: data.event.desc,
      };

      console.log(data.event, this.formUpdate);
      this.dialogVisible = true;
    },

    handleEvents(events) {
      this.currentEvents = events;
    },
  },
};

// const isEmptyObject = v => {
//   return !!v && v.constructor === Object && Object.keys(v).length === 0;
// };
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}

h2 {
  margin: 0;
  font-size: 16px;
}

ul {
  margin: 0;
  padding: 0 0 0 1.5em;
}

li {
  margin: 1.5em 0;
  padding: 0;
}

b {
  /* used for event dates/times */
  margin-right: 3px;
}

.demo-app {
  display: flex;
  min-height: 100%;
  font-family: Arial, Helvetica Neue, Helvetica, sans-serif;
  font-size: 14px;
}

.demo-app-sidebar {
  width: 300px;
  line-height: 1.5;
  background: #eaf9ff;
  border-right: 1px solid #d3e2e8;
}

.demo-app-sidebar-section {
  padding: 2em;
}

.demo-app-main {
  flex-grow: 1;
  padding: 3em;
}

.fc {
  /* the calendar root */
  max-width: 1100px;
  margin: 0 auto;
}
</style>


