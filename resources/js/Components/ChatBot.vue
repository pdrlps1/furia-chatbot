<template>
    <div class="chat-container">
        <div v-for="msg in messages" :key="msg.id" :class="msg.sender">
            {{ msg.text }}
        </div>
        <input v-model="input" @keyup.enter="sendMessage" placeholder="Digite sua mensagem..." />
    </div>
</template>

<script>
export default {
    data() {
        return {
            messages: [
                { id: 1, sender: 'bot', text: 'Fala, FURIOSO! O que quer saber hoje?' }
            ],
            input: ''
        };
    },
    methods: {
        sendMessage() {
            if (!this.input) return;
            this.messages.push({ id: Date.now(), sender: 'user', text: this.input });
            // Chama API Laravel:
            fetch('/api/chat', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ question: this.input })
            })
                .then(res => res.json())
                .then(data => {
                    this.messages.push({ id: Date.now(), sender: 'bot', text: data.reply });
                    this.input = '';
                });
        }
    }
}
</script>
<style>
/* estilos simples para chat */
.chat-container {
    max-width: 400px;
    margin: auto;
    padding: 20px;
    background: #181818;
    color: #fff;
    border-radius: 8px;
}

.bot {
    text-align: left;
    margin: 10px 0;
}

.user {
    text-align: right;
    margin: 10px 0;
    color: #CA8795;
}

input {
    width: 100%;
    padding: 10px;
    border-radius: 5px;
    border: none;
    margin-top: 10px;
}
</style>