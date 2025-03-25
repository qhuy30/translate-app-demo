﻿# Translate App Demo

## 1. Khởi tạo dự án

Tạo một dự án mới với Expo:
```sh
npx create-expo-app translate-app --template blank
```

## 2. Cài đặt dependencies

Cài đặt các thư viện cần thiết:
```sh
npm install @react-native-picker/picker expo-speech expo-clipboard
```

## 3. Cấu trúc dự án

![alt text](image-1.png)

## 4. Chạy ứng dụng
Tôi sẽ hướng dẫn bạn từng bước đầu tiên để bắt đầu xây dựng ứng dụng:
Bước 1: Cài đặt các dependencies cần thiết
```sh
expo install expo-speech expo-clipboard
```

Bước 2: Tạo cấu trúc thư mục cơ bản
```sh
mkdir src
mkdir src\components
mkdir src\constants
mkdir src\utils
```

Bước 3: Tạo file constants đầu tiên
1. Tạo file src/constants/languages.js
```sh
export const LANGUAGES = [
  { code: 'en', name: 'English' },
  { code: 'vi', name: 'Vietnamese' },
  { code: 'ja', name: 'Japanese' },
  { code: 'ko', name: 'Korean' },
  { code: 'zh', name: 'Chinese' },
  // Thêm các ngôn ngữ khác tùy nhu cầu
];
```
2. Tạo file src/constants/styles.js
```sh
import { StyleSheet } from 'react-native';

export const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f8f8f8',
  },
  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    padding: 16,
  },
  headerTitle: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#4B0082',
  },
  // Thêm các styles cơ bản khác
});
```
3. Tạo file App.js cơ bản đầu tiên
```sh
import React, { useState } from 'react';
import { SafeAreaView, StatusBar, View, Text } from 'react-native';
import { styles } from './src/constants/styles';

export default function App() {
  // Khởi tạo các state cơ bản
  const [text, setText] = useState("");
  const [translatedText, setTranslatedText] = useState("");
  const [sourceLang, setSourceLang] = useState("en");
  const [targetLang, setTargetLang] = useState("vi");

  return (
    <SafeAreaView style={styles.container}>
      <StatusBar backgroundColor="#f8f8f8" barStyle="dark-content" />
      
      {/* Header */}
      <View style={styles.header}>
        <Text style={styles.headerTitle}>Quick Translator</Text>
      </View>
      
      {/* Các component khác sẽ được thêm sau */}
    </SafeAreaView>
  );
}
```

Bước 4: Tạo component đầu tiên - TranslationInput
Tạo file src/components/TranslationInput.js
```sh
import React from 'react';
import { View, TextInput, TouchableOpacity, Text, StyleSheet } from 'react-native';

export const TranslationInput = ({ text, setText, clearText }) => {
  return (
    <View style={styles.container}>
      <TextInput
        style={styles.input}
        multiline
        placeholder="Nhập văn bản cần dịch..."
        value={text}
        onChangeText={setText}
      />
      {text.length > 0 && (
        <TouchableOpacity style={styles.clearButton} onPress={clearText}>
          <Text style={styles.clearButtonText}>×</Text>
        </TouchableOpacity>
      )}
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    margin: 16,
    position: 'relative',
  },
  input: {
    borderWidth: 1,
    borderColor: '#ddd',
    borderRadius: 8,
    padding: 12,
    minHeight: 100,
    backgroundColor: '#fff',
    fontSize: 16,
  },
  clearButton: {
    position: 'absolute',
    right: 8,
    top: 8,
    padding: 4,
  },
  clearButtonText: {
    fontSize: 20,
    color: '#666',
  },
});
```

## 5. Chạy ứng dụng

Khởi chạy ứng dụng trên thiết bị hoặc trình giả lập:
```sh
npx expo start
```

## 6. Chức năng chính

- Chọn ngôn ngữ dịch với `@react-native-picker/picker`
- Đọc văn bản bằng `expo-speech`
- Sao chép văn bản dịch bằng `expo-clipboard`

## 7. Liên hệ
Nếu có bất kỳ vấn đề nào, vui lòng tạo issue trên GitHub hoặc liên hệ với nhóm phát triển.

