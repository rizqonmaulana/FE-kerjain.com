<template>
  <div class="form-hire">
    <b-form @submit.prevent="sendHiring">
      <p class="form-title">Hubungi {{ userId.user_name }}</p>
      <p class="form-desc">
        Hubungi {{ userId.user_name }} untuk memberikan penawaran pekerjaan /
        project
      </p>
      <div class="perpose">
        <p class="perpose-title">Tujuan tentang Pesan Ini</p>
        <select class="perpose-select" v-model="form.perpose">
          <option value="Project">Project</option>
          <option value="Job Invitation">Job Invitation</option>
        </select>
      </div>
      <div class="name-hiring">
        <p class="title">Nama Lengkap</p>
        <input
          class="input-name"
          type="text"
          v-model="form.name"
          placeholder="Masukan Nama Lengkap"
        />
      </div>
      <div class="name-hiring">
        <p class="title">Email</p>
        <input
          class="input-name"
          type="email"
          v-model="form.email"
          placeholder="Masukan Email"
        />
      </div>
      <div class="name-hiring">
        <p class="title">No Handphone</p>
        <input
          class="input-name"
          type="text"
          v-model="form.phone"
          placeholder="Masukan no handphone"
        />
      </div>
      <div class="name-hiring">
        <p class="title">Deskripsi</p>
        <b-form-textarea
          id="textarea"
          class="deskriptsi"
          v-model="form.deskripsi"
          placeholder="Deskripsikan Lebih Detail"
          rows="6"
          max-rows="9"
        ></b-form-textarea>
      </div>
      <div class="button-hire">
        <button class="hire-button">Hire</button>
      </div>
    </b-form>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'
import { alert } from '../../mixins/alert'
export default {
  name: 'formHire',
  mixins: [alert],
  data() {
    return {
      form: {
        perpose: '',
        name: '',
        email: '',
        phone: '',
        deskripsi: '',
        user_id_to: 0
      }
    }
  },
  computed: {
    ...mapGetters({ userId: 'getUserById' })
  },
  methods: {
    ...mapActions(['sendJobInvitations', 'postNotif', 'getUserFrom']),
    sendHiring() {
      this.form.user_id_to = this.$route.params.id
      this.sendJobInvitations(this.form)
        .then(result => {
          const setData = {
            notif_to_id: this.form.user_id_to,
            notif_from: this.form.name,
            notif_purpose: this.form.perpose
          }
          this.postNotif(setData)
          this.successAlert(result.msg)
        })
        .catch(err => {
          this.errorAlert(err.data.msg)
        })
    },
    getUser() {
      this.getUserFrom(this.$route.params.id)
    }
  },
  created() {
    this.getUser()
  }
}
</script>

<style scoped>
.form-hire {
  margin-top: 150px;
  font-family: 'Open Sans';
  margin-left: 60px;
}
.form-title {
  font-size: 32px;
  font-weight: 600;
}
.form-desc {
  font-weight: 400;
  font-size: 16px;
  color: #46505c;
}
.perpose-title {
  font-size: 12px;
  color: #9ea0a5;
}
.perpose-select {
  margin-top: -15px;
  width: 100%;
  height: 40px;
  padding-left: 4%;
  border-radius: 5px;
  border: 1px solid #e2e5ed;
}
.name-hiring {
  margin-top: 32px;
}
.title {
  font-size: 12px;
  color: #9ea0a5;
}
.input-name {
  margin-top: -15px;
  width: 100%;
  height: 40px;
  padding-left: 4%;
  border-radius: 5px;
  border: 1px solid #e2e5ed;
  color: #9ea0a5;
}
.deskripsi {
  padding-left: 4%;
  color: #9ea0a5;
}
.button-hire {
  margin-top: 52px;
}
.hire-button {
  width: 100%;
  height: 50px;
  border: none;
  background-color: #fbb017;
  color: white;
  font-weight: 700;
  border-radius: 10px;
}
input:focus {
  background-color: white;
  color: black;
}
@media only screen and (max-width: 600px) {
  .form-hire {
    margin-top: 150px;
    font-family: 'Open Sans';
    margin-left: 0px;
  }
}
</style>
