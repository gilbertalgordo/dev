import React, { useState, useCallback } from 'react';
import { View, StyleSheet } from 'react-native';
import { GiftedChat } from 'react-native-gifted-chat';
import { sendMessageToChatGPT } from './src/ChatGPTService';

const App = () => {
  const [messages, setMessages] = useState([]);

  const handleSend = useCallback(async (newMessages = []) => {
    setMessages((previousMessages) => GiftedChat.append(previousMessages, newMessages));

    const userMessage = newMessages[0];
    const formattedMessages = [
      { role: 'system', content: 'You are a helpful assistant.' },
      ...messages.map((msg) => ({ role: msg.user._id === 1 ? 'user' : 'assistant', content: msg.text })),
      { role: 'user', content: userMessage.text },
    ];

    const responseMessage = await sendMessageToChatGPT(formattedMessages);

    if (responseMessage) {
      const botMessage = {
        _id: Math.random().toString(36).substring(7),
        text: responseMessage.content,
        createdAt: new Date(),
        user: { _id: 2, name: 'ChatGPT' },
      };
      setMessages((previousMessages) => GiftedChat.append(previousMessages, [botMessage]));
    }
  }, [messages]);

  return (
    <View style={styles.container}>
      <GiftedChat messages={messages} onSend={(newMessages) => handleSend(newMessages)} user={{ _id: 1 }} />
    </View>
  );
};

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: '#fff' },
});

export default App;
