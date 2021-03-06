<template>
  <div class="booker">
    <div class="chat">
      <div class="container">
        <div class="chat-page">
          <div class="msg-inbox">
            <div class="chats" id="chats">
              <div class="msg-page" id="msg-page">
                <div v-if="loadingMessages" class="loading-messages-container">
                  <spinner :size="100" />
                  <span class="loading-text">Loading Messages</span>
                </div>
                <div class="text-center img-fluid empty-chat" v-else-if="!messages.length">
                  <div class="empty-chat-holder">
                    <img src="../assets/empty-state.svg" class="img-res" alt="empty chat image" />
                  </div>

                  <div>
                    <h2>No new message?</h2>
                    <h6 class="empty-chat-sub-title">Send your first message below.</h6>
                  </div>
                </div>

                <div v-else>
                  <div v-for="message in messages" v-bind:key="message.id">
                    <div class="received-chats" v-if="message.receiverId == userUID ">
                      <div class="received-chats-img">
                        <img v-bind:src="message.sender.avatar" alt class="avatar" />
                      </div>

                      <div class="received-msg">
                        <div class="received-msg-inbox">
                          <p>
                            <span>{{ message.sender.name }}</span>
                            <br />
                            {{ message.text }}
                          </p>
                        </div>
                      </div>
                    </div>

                    <div class="outgoing-chats" v-else>
                      <div class="outgoing-chats-msg">
                        <p>{{ message.text }}</p>
                      </div>

                      <div class="outgoing-chats-img">
                        <img v-bind:src="message.sender.avatar" alt class="avatar" />
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <div class="msg-bottom">
            <form class="message-form" v-on:submit.prevent="sendGroupMessage">
              <div class="input-group">
                <input
                  type="text"
                  class="form-control message-input"
                  placeholder="Type something"
                  v-model="chatMessage"
                  required
                />
                <spinner v-if="sendingMessage" class="sending-message-spinner" :size="30" />
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { CometChat } from "@cometchat-pro/chat";
import Spinner from "../components/Spinner.vue";
export default {
  name: "Chat",
  components: {
    Spinner
  },
  props: {
    receiverID: String,
    userUID: String
  },
  data() {
    return {
      sendingMessage: false,
      chatMessage: "",
      // groupMessages: [],
      loadingMessages: false,
      messages: []
    };
  },
  mounted() {
    console.log('uid is ', this.userUID);
    console.log('other id is ', this.receiverID);
          this.loadingMessages = true;
  },
  methods: {
    getMessages() {
      console.log('enother fetching')
      var listenerID = "UNIQUE_LISTENER_ID";
      const messagesRequest = new CometChat.MessagesRequestBuilder()
        .setLimit(100)
        .build();
      messagesRequest.fetchPrevious().then(
        messages => {
          console.log("Message list fetched:", messages);
          messages.forEach(message => this.checkIfChat(message));
          // this.groupMessages = [...this.groupMessages, ...messages];
          this.loadingMessages = false;
          this.$nextTick(() => {
            this.scrollToBottom();
          });
        },
        error => {
          console.log("Message fetching failed with error:", error);
          // location.reload();
        }
      );
      CometChat.addMessageListener(
        listenerID,
        new CometChat.MessageListener({
          onTextMessageReceived: textMessage => {
            console.log("Text message received successfully", textMessage);
            // Handle text message
            this.messages = [...this.messages, textMessage];
            this.loadingMessages = false;
            this.$nextTick(() => {
              this.scrollToBottom();
            });
          }
        })
      );
    },
    checkIfChat(message) {
      if (message.category == "message") {
        if (
          message.receiverId == this.receiverID ||
          message.sender.uid == this.receiverID
        ) {
          this.messages.push(message);
        }
      }
    },
    sendGroupMessage() {
      this.sendingMessage = true;
      var messageText = this.chatMessage;
      var messageType = CometChat.MESSAGE_TYPE.TEXT;
      var receiverType = CometChat.RECEIVER_TYPE.USER;
      let globalContext = this;

      var textMessage = new CometChat.TextMessage(
        this.receiverID,
        messageText,
        receiverType
      );
      CometChat.sendMessage(textMessage).then(
        message => {
          console.log("Message sent successfully:", message);
          this.chatMessage = "";
          this.sendingMessage = false;
          // Text Message Sent Successfully
          this.messages = [...globalContext.messages, message];
          this.$nextTick(() => {
            this.scrollToBottom();
          });
          console.log(this.groupMessages);
        },
        error => {
          console.log("Message sending failed with error:", error);
        }
      );
    },
    scrollToBottom() {
      const chat = document.getElementById("msg-page");
      chat.scrollTo(0, chat.scrollHeight + 30);
    }
  }
};
</script>
<style scoped>
* {
  box-sizing: border-box;
}

body {
  color: #333;
  font-size: 13px;
  margin: 0;
  width: 100%;
  height: 100vh;
  -webkit-font-smoothing: antialiased;
}

h3 {
  font-family: "Abril Fatface";
  margin-bottom: 20px;
}

label {
  color: #c9d4d8;
  font-weight: bold;
}

.form-wrapper label {
  margin: 0;
  color: #bfcdd8;
}

.form-wrapper #username {
  border-bottom: 2px solid #e6ecee !important;
  border-radius: 0 !important;
  padding-top: 0.375rem;
  padding-right: 0.75rem;
  padding-bottom: 0.375rem;
  padding-left: 0;
}

.form-wrapper i {
  position: absolute;
  bottom: 9px;
  right: 0;
}

.chat {
  flex: 1;
  background-repeat: no-repeat;
  background-position: center top;
  background-size: cover;
  width: 100%;
}

.empty-chat-holder {
  width: 100%;
  height: 250px;
  margin-top: 70px;
}

.empty-chat {
  position: relative;
  margin: auto;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  width: 100%;
}

.empty-chat h2 {
  color: #1546dc;
  font-family: "Abril Fatface";
  padding: 0;
  font-size: 2rem;
  margin: 25px auto 15px;
}

.empty-chat-holder img {
  width: 100%;
  height: 100%;
}

.avatar {
  width: 37px;
  height: 37px;
}

.container {
  margin: auto;
  max-width: 800px;
  width: calc(100% - 20px);
  height: 588px;
  font-family: sans-serif;
  letter-spacing: 0.5px;
  background: #f8f9fb;
  padding: 0 !important;
  border-radius: 7px;
  box-sizing: border-box;
}

.active {
  font-family: "Roboto";
  width: 120px;
  float: left;
}

.active h5 {
  padding: 10px;
  color: #444;
  font-size: 18px;
  font-weight: 500;
  margin: 0;
}

.active h6 {
  font-size: 10px;
  line-height: 2px;
  color: #fff;
}

.header-icons {
  width: 120px;
  float: right;
  margin-right: 10px;
}

.header-icons span {
  line-height: 70px;
  cursor: pointer;
  font-size: 14px;
}

.header-icons .fa {
  cursor: pointer;
  margin: 10px;
}

.msg-page {
  height: 516px;
  overflow-y: auto;
  padding-bottom: 50px;
}

.received-chats {
  padding: 20px;
  display: flex;
  justify-content: flex-start;
}

.received-chats-img {
  display: inline-block;
  display: flex;
  align-items: center;
}

.received-msg {
  display: inline-block;
  padding: 0;
  vertical-align: top;
}

.received-msg-inbox {
  width: 57%;
}

.received-msg-inbox p {
  font-family: "Roboto";
  background: #ffffff none repeat scroll 0 0;
  border-radius: 7px;
  color: #646464;
  font-size: 16px;
  margin: 0;
  padding: 12px;
  position: relative;
  width: 253px;
  min-height: 61px;
  left: 30px;
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.1);
}

.received-msg-inbox p span {
  color: #6889fd;
  text-transform: capitalize;
  font-weight: bold;
  font-size: 14px;
}

.received-msg-inbox p:after {
  content: "";
  position: absolute;
  left: 0;
  top: 50%;
  width: 0;
  height: 0;
  border: 10px solid transparent;
  border-right-color: #ffffff;
  border-left: 0;
  margin-top: -10px;
  margin-left: -10px;
  transform: skew(0, 30deg);
}

#triangle-right {
  width: 0;
  height: 0;
  border-top: 50px solid transparent;
  border-left: 100px solid red;
  border-bottom: 50px solid transparent;
}

.time {
  color: #777;
  display: block;
  font-size: 12px;
  margin: 8px 0 0;
}

.outgoing-chats {
  overflow: hidden;
  margin: 26px 20px;
  display: flex;
  justify-content: flex-end;
}

.outgoing-chats-msg p {
  font-family: "Roboto";
  background: #2296f3 none repeat scroll 0 0;
  color: #fff;
  font-size: 16px;
  margin: 0;
  color: #fff;
  padding: 12px;
  width: 253px;
  min-height: 61px;
  position: relative;
  border-radius: 7px;
  display: flex;
  align-items: center;
  line-height: 1.2;
}

.outgoing-chats-msg p::before {
  content: "";
  position: absolute;
  right: 0;
  top: 50%;
  width: 0;
  height: 0;
  border: 10px solid transparent;
  border-left-color: #2296f3;
  border-right: 0;
  margin-top: -10px;
  margin-right: -10px;
  transform: skew(0, -30deg);
}

.outgoing-chats-img {
  display: flex;
  align-items: center;
  padding-top: 10px;
  margin-left: 20px;
}

.msg-bottom {
  position: relative;
  height: 60px;
  background-color: #007bff;
  border-radius: 100px 100px 0 0;
}

.input-group {
  margin-right: 20px;
  border-top: 1px solid #dee6eb;
  width: 100% !important;
  background-color: #fff;
  height: 100%;
  border-radius: 0 0 7px 7px;
}

.input-group input {
  height: 100%;
  background: #fff;
}

.input-group ::placeholder {
  color: #c0c0c0 !important;
}

input.form-control.message-input {
  width: 100%;
}

.form-control {
  border: none !important;
  border-radius: 20px !important;
}

.input-group-text {
  background: transparent !important;
  border: none !important;
}

.input-group .fa {
  color: #007bff;
  float: right;
}

.bottom-icons {
  float: left;
  margin-top: 17px;
  width: 30% !important;
  margin-left: 22px;
}

.bottom-icons .fa {
  color: #fff;
  padding: 5px;
}

.form-control:focus {
  border-color: none !important;
  box-shadow: none !important;
  border-radius: 20px;
}

.msg-bottom {
  border-radius: 0px 0px 10px 10px;
  position: absolute;
  z-index: 10;
  left: 0;
  bottom: 0;
  width: 100%;
}

.message-form {
  height: 100%;
}

.chat-page {
  position: relative;
}

.nav-right-section {
  display: flex;
  align-items: center;
}

nav .spinner {
  margin-left: 15px;
  width: 64px;
}

.sending-message-spinner {
  margin-right: 20px;
}

.message-input {
  padding: 6px 20px;
}

.empty-chat-sub-title {
  text-align: center;
  color: #555;
  font-size: 18px;
}

.loading-messages-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin-top: 150px;
}

.loading-text {
  font-family: "Roboto";
  color: #1546dc;
  font-size: 25px;
  margin-top: 20px;
}

.loading-messages-container .spinner svg {
  stroke: #ccd7f0;
}

@media (max-width: 450px) {
  .outgoing-chats-img {
    display: none;
  }
  .received-chats-img {
    display: none;
  }
  .received-msg-inbox p {
    left: 0;
    width: 100%;
  }
  .received-msg-inbox p:after {
    display: none;
  }
  .received-msg-inbox {
    width: 100%;
  }
  .received-msg {
    width: 100%;
  }
  .outgoing-chats-msg {
    width: 100%;
  }
  .outgoing-chats-msg p {
    width: 100%;
  }
  .outgoing-chats-msg p::before {
    display: none;
  }
}

.booker {
  display: flex;
  flex-direction: column;
  flex: 1;
}
</style>