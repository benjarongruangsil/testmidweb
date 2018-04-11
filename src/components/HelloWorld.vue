<template>
  <div class="hello">

                  <a class="button is-info is-focused" @click="insert()">Add</a>

          <br>

          <div class="columns">
            <div class="column"></div>
              <div class="column is-11 is-offset-1">
                <div  class="tablestudent" >
                  <table class="table" >
                    <thead>
                      <tr>
                        <th scope="col">#</th>
                        <th scope="col">รูป</th>
                        <th scope="col">สถานะ</th>
                        <th scope="col">delete</th>

                      </tr>
                    </thead>
                    <tbody  v-for = "(show, key, count) in showstudent"  :key ="show.num">
                      <tr>
                        <td>  {{count+1}}  </td>
                        <td>  <img width="10%" src="../assets/aa.jpeg">  </td>
                       <td>  <a  v-if ="show.type === 'true'">    <!-- แสดง เครื่องซักผ้าที่ใช้ไดด้ -->
                                  <a class="button  is-link" @click="truee(key)">ใช้งานได้</a>
                                  <a class="button " @click="falsee(key)">พัง</a>
                              </a>
                              <a  v-if ="show.type === 'false'">    <!-- แสดง เครื่องซักผ้าที่พัง -->
                                        <a class="button" @click="truee(key)">ใช้งานได้</a>
                                        <a class="button  is-danger " @click="falsee(key)">พัง</a>
                              </a>
                        </td>
                        <td>      <a class="button is-danger is-outlined" @click="Delete(key)">Delete</a> </td>
                      </tr>
                    </tbody>
                  </table>
              </div>
            </div>
          </div>
  </div>
</template>

<script>
import firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyDcvOxf7GThb3ENlkIT24V5skXlGA6N3DQ',
  authDomain: 'midterm-63133.firebaseapp.com',
  databaseURL: 'https://midterm-63133.firebaseio.com',
  projectId: 'midterm-63133',
  storageBucket: 'midterm-63133.appspot.com',
  messagingSenderId: '682149150463'
}
firebase.initializeApp(config)
export default {
  name: 'HelloWorld',
  data () {
    return {
      showstudent: '',
      data: {
        type: ''
      }
    }
  },
  created: function () {
    this.pullData()
  },
  methods: {
    pullData: function () {
      let that = this
      firebase.database().ref('/student/').once('value').then(function (snapshot) {
        that.showstudent = snapshot.val()
      })
    },
    insert () { // เพิ่มเครื่องซักผ้า โดยก้แล้ว จะให้ใส่คำว่า true ไปเลย 
      this.data.type = 'true'
      firebase.database().ref('student').push(this.data)
      this.pullData()
    },
    truee (key, type) { // เปลี่ยนค่าจาก false เป็น true
      firebase.database().ref('/student/').child(key).update({
        type: 'true'
      })
      this.pullData()
      this.checkEdit = ''
    },
    falsee (key, type) {  // เปลี่ยนค่าจาก true เป็น false
      firebase.database().ref('/student/').child(key).update({
        type: 'false'
      })
      this.pullData()
      this.checkEdit = ''
    },
    Delete (key) {
      firebase.database().ref('student').child(key).remove()
      this.pullData()
    }
  }
}

</script>
