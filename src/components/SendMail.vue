<template>
  <v-form ref="form">
    <v-container>
      <v-row>
        <v-col cols="6" md="3">
          <v-text-field v-model="form.from" :rules="form.fromRule" label="보내는사람" required />
        </v-col>

        <v-col cols="6" md="3">
          <v-text-field v-model="form.to" :rules="form.toRule" label="받는사람" required />
        </v-col>
      </v-row>
      <v-text-field v-model="form.subject" :rules="form.subjectRule" label="제목" required />
      <div v-if="!template">
        <v-textarea label="본문" v-model="form.text" />
      </div>
      <div v-else>
        <v-label>초대 템플릿</v-label>
        <v-row>
          <v-col cols="6" md="3">
            <v-text-field
              v-model="invitation.title"
              :rules="invitation.titleRule"
              label="제목"
              required
            />
          </v-col>
          <v-col cols="6" md="3">
            <v-text-field
              v-model="invitation.discord"
              :rules="invitation.discordRule"
              label="디스코드"
              required
            />
          </v-col>
        </v-row>
        <v-textarea label="본문" v-model="invitation.content" :rules="invitation.contentRule" />
      </div>
      <div class="d-flex">
        <v-btn color="success" class="mr-4" @click="sendMail">SendMail</v-btn>
        <v-btn color="warning" class="mr-4" @click="template = !template">
          {{
          template ? 'send Only Text' : ' Send With Template'
          }}
        </v-btn>
      </div>
    </v-container>
  </v-form>
</template>

<script>
import Axios from 'axios'
import { config } from './../../config'

export default {
  name: 'SendMail',
  data: () => ({
    template: false,
    form: {
      from: 'Team IF <no-reply@teamif.io>',
      fromRule: [v => !!v || '보내는사람은 무조건 적어야합니다'],
      to: '',
      toRule: [v => !!v || '받는사람은 무조건 적어야합니다'],
      subject: '',
      subjectRule: [v => !!v || '제목은 무조건 작성하여야 합니다'],
      text: ''
    },
    invitation: {
      title: '',
      titleRule: [v => !!v || '타이틀은 무조건 적어야합니다'],
      content: '',
      contentRule: [v => !!v || '내용은 무조건 적어야합니다'],
      discord: '',
      discordRule: [v => !!v || '디스코드 태그는 무조건 적어야합니다']
    }
  }),
  methods: {
    sendMail() {
      this.$refs.form.validate()
      if (this.template) {
        Axios.post(
          'https://shol.xyz:8446/mail/invitation',
          {
            from: this.form.from,
            to: this.form.to,
            subject: this.form.subject,
            title: this.invitation.title,
            discord: this.invitation.discord,
            content: this.invitation.content
          },
          {
            auth: {
              username: 'admin',
              password: config.password
            }
          }
        )
          .then(result => {
            console.log(result.data)
            alert(
              '메일이 전송되었습니다.\nctrl+shift+i를 눌러 콘솔에서 로그를 확인하세요'
            )
          })
          .catch(err => {
            console.log(err)
            alert('에러가 발생하였습니다. 콘솔에서 확인해주세요')
          })
      } else {
        Axios.post(
          'http://localhost:8446/mail',
          {
            from: this.form.from,
            to: this.form.to,
            subject: this.form.subject,
            text: this.form.text
          },
          {
            auth: {
              username: 'admin',
              password: config.password
            }
          }
        )
          .then(result => {
            console.log(result.data)
            alert(
              '메일이 전송되었습니다.\nctrl+shift+i를 눌러 콘솔에서 로그를 확인하세요'
            )
          })
          .catch(err => {
            console.log(err)
            alert('에러가 발생하였습니다. 콘솔에서 확인해주세요')
          })
      }
    }
  }
}
</script>
