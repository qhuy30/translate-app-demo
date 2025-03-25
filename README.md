# Translate App Demo

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

```
translate-app/
├── App.js
├── package.json
├── node_modules/
├── assets/
├── components/
└── screens/
```

## 4. Chạy ứng dụng

Khởi chạy ứng dụng trên thiết bị hoặc trình giả lập:
```sh
npx expo start
```

## 5. Chức năng chính

- Chọn ngôn ngữ dịch với `@react-native-picker/picker`
- Đọc văn bản bằng `expo-speech`
- Sao chép văn bản dịch bằng `expo-clipboard`

## 6. Liên hệ
Nếu có bất kỳ vấn đề nào, vui lòng tạo issue trên GitHub hoặc liên hệ với nhóm phát triển.

