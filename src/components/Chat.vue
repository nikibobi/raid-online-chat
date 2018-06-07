<template>
    <div>
        <ul ref="chat">
            <li v-for="message in messages" :key="message.key">
                <span :style="senderStyle(message.sender)" >{{ message.sender }}</span>: {{ message.text }}
            </li>
        </ul>
        <input type="text" v-model="newMessage" @keypress.enter="send" />
    </div>
</template>

<script>
export default {
    name: "Chat",
    props: ["server", "token", "username"],
    data() {
        return {
            path: "/hubs/chat",
            messages: [],
            newMessage: null
        };
    },
    created() {
        this.connection = new signalR.HubConnectionBuilder()
                                .withUrl(this.server + this.path, {
                                    transport: 1,
                                    accessTokenFactory: () => this.token,
                                    skipNegotiation: true
                                })
                                .build();
    },
    mounted() {
        this.connection.start();
        this.connection.on("Message", (sender, text) => {
            this.messages.push({ sender, text });
        });
    },
    watch: {
        messages: function() {
            this.$nextTick(function() {
                const container = this.$refs["chat"];
                container.scrollTop = container.scrollHeight;
            });
        }
    },
    methods: {
        send() {
            this.connection.send("Send", this.newMessage);
            this.newMessage = null;
        },
        senderStyle(sender) {
            return {
                color: (sender == this.username ? "yellow" : "navy")
            };
        }
    }
}
</script>

<style scoped>
ul {
    background-color: #3c3a44;
    list-style: none;
    padding: 4px;
    margin: 0;
    height: 228px;
    width: 284px;
    overflow-y: auto;
    font-family: Fontin;
    font-size: 12px;
}

li {
    color: white;
    word-break: normal;
}

input {
    color: white;
    background: #2c2a32;
    box-sizing: border-box;
    width: 100%;
    font-family: Fontin;
    padding: 4px;
    /* remove focused outline */
    outline-style: none;
    box-shadow: none;
    border-color: transparent;
}
</style>
