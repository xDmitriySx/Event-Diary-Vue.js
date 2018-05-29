<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Firebase Events Diary on Vue.js</h1>
    </div>

    <div class="card">
      <div class="card-header">
        <h3>Add new event</h3>
      </div>
      <div class="card-body">
        <form id="form" class="row" v-on:submit.prevent="addEvent">
        <div class="col">
          <label for="eventTitle"> <strong> Name of your event </strong> </label>
          <input type="text" id="eventTitle" class="form-control" v-model="newEvent.title">
        </div>
        <div class="col">
          <label for=eventDate> <strong> Date of your event </strong> </label>
          <input type="date" id="eventDate" class="form-control" v-model="newEvent.date">
        </div>
        <div class="col">
          <label for="eventTime"> <strong> Time  </strong> </label>
          <input type="time" id="eventTime" class="form-control" v-model="newEvent.time">
        </div>
        <div class="col">
        <input type="submit" id="button" class="btn btn-primary" value="Add Event">
        </div>
        </form>
      </div>
    </div>

    <div class="card">
      <div class="card-header">
        <h3> Events List </h3>
      </div>
      <div class="card-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>
                Event
              </th>
              <th>
                Date
              </th>
              <th>
                Time
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="event in events">
              <td>
                {{event.title}}
              </td>
              <td>
                {{event.date}}
              </td>
              <td>
                {{event.time}}
              </td>
              <td>
                <span id="remove" v-on:click="removeEvent(event)"> <strong> &times </strong> </span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>

  import Firebase from 'firebase'

  let config = {
    apiKey: "AIzaSyBkZlRMreShOPSUKt8l123CGhC7Qgiz48M",
    authDomain: "events-diary-96a4b.firebaseapp.com",
    databaseURL: "https://events-diary-96a4b.firebaseio.com",
    projectId: "events-diary-96a4b",
    storageBucket: "events-diary-96a4b.appspot.com",
    messagingSenderId: "946863831629"
  }

  let app = Firebase.initializeApp(config);
  let db = app.database();

  let eventsRef = db.ref('events');


export default {
  name: 'app',
  firebase: {
    events: eventsRef
  },
  data(){
    return {
      newEvent: {
        title: '',
        date: '',
        time: ''
      }
    }
  },
  methods: {
    addEvent: function(){
      if(this.newEvent.title != ''){
      eventsRef.push(this.newEvent);
      this.newEvent.title = '';
      this.newEvent.date = '';
      this.newEvent.time = '';
      }
    },
    removeEvent: function(event){
      eventsRef.child(event['.key']).remove();
    }
  }
}
</script>

<style>

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

#button{
  margin-top: 30px;
}

#remove{
  cursor: pointer;
  color: #777777;
}

</style>
