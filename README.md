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

Bước 5: Tạo LanguageSelector Component
Tạo file src/components/LanguageSelector.js:
```sh
import React from 'react';
import { View, TouchableOpacity, Text, StyleSheet } from 'react-native';
import { LANGUAGES } from '../constants/languages';

export const LanguageSelector = ({
  sourceLang,
  targetLang,
  autoDetect,
  onSourcePress,
  onTargetPress,
  onSwapPress,
}) => {
  const getLanguageName = (code) => {
    return LANGUAGES.find(lang => lang.code === code)?.name || code;
  };

  return (
    <View style={styles.container}>
      <TouchableOpacity style={styles.langButton} onPress={onSourcePress}>
        <Text style={styles.langText}>
          {autoDetect ? "Tự động phát hiện" : getLanguageName(sourceLang)}
        </Text>
      </TouchableOpacity>

      <TouchableOpacity style={styles.swapButton} onPress={onSwapPress}>
        <Text style={styles.swapButtonText}>⇄</Text>
      </TouchableOpacity>

      <TouchableOpacity style={styles.langButton} onPress={onTargetPress}>
        <Text style={styles.langText}>{getLanguageName(targetLang)}</Text>
      </TouchableOpacity>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flexDirection: 'row',
    alignItems: 'center',
    justifyContent: 'center',
    padding: 16,
  },
  langButton: {
    padding: 10,
    borderRadius: 8,
    backgroundColor: '#fff',
    borderWidth: 1,
    borderColor: '#ddd',
    minWidth: 120,
    alignItems: 'center',
  },
  langText: {
    fontSize: 16,
    color: '#4B0082',
  },
  swapButton: {
    padding: 10,
    marginHorizontal: 10,
  },
  swapButtonText: {
    fontSize: 24,
    color: '#4B0082',
  },
});
```

Bước 8: Tạo Translation Utils
Tạo file src/utils/translation.js:
```sh
// Đây là ví dụ sử dụng Google Translate API
// Bạn cần thay thế bằng API key của riêng mình
export const translateText = async (text, sourceLang, targetLang) => {
  try {
    const response = await fetch(
      `https://translation-api-endpoint.com/translate`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': 'Bearer YOUR_API_KEY'
        },
        body: JSON.stringify({
          text,
          source: sourceLang,
          target: targetLang
        })
      }
    );
    
    const data = await response.json();
    return data.translatedText;
  } catch (error) {
    console.error('Translation error:', error);
    throw error;
  }
};

export const detectLanguage = async (text) => {
  try {
    const response = await fetch(
      `https://translation-api-endpoint.com/detect`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': 'Bearer YOUR_API_KEY'
        },
        body: JSON.stringify({ text })
      }
    );
    
    const data = await response.json();
    return data.detectedLanguage;
  } catch (error) {
    console.error('Language detection error:', error);
    throw error;
  }
};
```
Bước 9: Tạo TranslationResult Component
Tạo file src/components/TranslationResult.js:
```sh
import React from 'react';
import { View, Text, TouchableOpacity, StyleSheet } from 'react-native';

export const TranslationResult = ({
  translatedText,
  onSpeak,
  onCopy,
  onFavorite,
  isFavorite
}) => {
  return (
    <View style={styles.container}>
      <Text style={styles.resultText}>{translatedText}</Text>
      
      <View style={styles.actions}>
        <TouchableOpacity style={styles.actionButton} onPress={onSpeak}>
          <Text style={styles.actionButtonText}>🔊</Text>
        </TouchableOpacity>
        
        <TouchableOpacity style={styles.actionButton} onPress={onCopy}>
          <Text style={styles.actionButtonText}>📋</Text>
        </TouchableOpacity>
        
        <TouchableOpacity style={styles.actionButton} onPress={onFavorite}>
          <Text style={styles.actionButtonText}>
            {isFavorite ? '⭐' : '☆'}
          </Text>
        </TouchableOpacity>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    margin: 16,
    padding: 16,
    backgroundColor: '#fff',
    borderRadius: 8,
    borderWidth: 1,
    borderColor: '#ddd',
  },
  resultText: {
    fontSize: 16,
    lineHeight: 24,
    color: '#333',
  },
  actions: {
    flexDirection: 'row',
    justifyContent: 'flex-end',
    marginTop: 16,
  },
  actionButton: {
    padding: 8,
    marginLeft: 16,
  },
  actionButtonText: {
    fontSize: 20,
  },
});
```
Bước 10: Cập nhật App.js để tích hợp các components mới
Cập nhật file App.js để thêm các components mới:
```sh
import React, { useState } from 'react';
import { SafeAreaView, StatusBar, View, Text } from 'react-native';
import { styles } from './src/constants/style';
import { LanguageSelector } from './src/components/LanguageSelector';
import { TranslationInput } from './src/components/TranslationInput';
import { TranslationResult } from './src/components/TranslationResult';

export default function App() {
  // Khởi tạo state
  const [text, setText] = useState("");
  const [translatedText, setTranslatedText] = useState("");
  const [sourceLang, setSourceLang] = useState("en");
  const [targetLang, setTargetLang] = useState("vi");
  const [autoDetect, setAutoDetect] = useState(false);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const swapLanguages = () => {
    setSourceLang(targetLang);
    setTargetLang(sourceLang);
  };

  const speakTranslation = () => {
    // TODO: Thêm logic text-to-speech
  };

  const copyToClipboard = () => {
    // TODO: Thêm logic copy văn bản
  };

  const translateText = async (text, sourceLang, targetLang) => {
    // TODO: Gọi API dịch
    return `Bản dịch của: ${text}`;
  };

  const handleDetectLanguage = async () => {
    // TODO: Tự động nhận diện ngôn ngữ
  };

  const handleTranslate = async () => {
    if (!text.trim()) return;

    setIsLoading(true);
    try {
      if (autoDetect) {
        await handleDetectLanguage();
      }

      const result = await translateText(text, sourceLang, targetLang);
      setTranslatedText(result);
    } catch (error) {
      setError("Translation failed");
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <SafeAreaView style={styles.container}>
      <StatusBar backgroundColor="#f8f8f8" barStyle="dark-content" />

      {/* Header */}
      <View style={styles.header}>
        <Text style={styles.headerTitle}>Quick Translator</Text>
      </View>

      <LanguageSelector
        sourceLang={sourceLang}
        targetLang={targetLang}
        autoDetect={autoDetect}
        onSourcePress={() => {/* TODO */}}
        onTargetPress={() => {/* TODO */}}
        onSwapPress={swapLanguages}
      />

      <TranslationInput
        text={text}
        setText={setText}
        clearText={() => setText("")}
      />

      {translatedText && (
        <TranslationResult
          translatedText={translatedText}
          onSpeak={speakTranslation}
          onCopy={copyToClipboard}
          onFavorite={() => {/* TODO */}}
          isFavorite={false}
        />
      )}
    </SafeAreaView>
  );
}
```

Bước 11: Tạo LanguagePicker Modal
Tạo file src/components/LanguagePicker.js:
```sh
import React from 'react';
import { 
  Modal, 
  View, 
  FlatList, 
  TouchableOpacity, 
  Text, 
  StyleSheet,
  SafeAreaView 
} from 'react-native';
import { LANGUAGES } from '../constants/languages';

export const LanguagePicker = ({
  visible,
  pickerType,
  onClose,
  onSelect,
  onAutoDetect,
  sourceLang,
  targetLang
}) => {
  const renderLanguageItem = ({ item }) => (
    <TouchableOpacity 
      style={styles.languageItem} 
      onPress={() => onSelect(item.code)}
    >
      <Text style={styles.languageText}>{item.name}</Text>
      {((pickerType === 'source' && item.code === sourceLang) ||
        (pickerType === 'target' && item.code === targetLang)) && (
        <Text style={styles.checkmark}>✓</Text>
      )}
    </TouchableOpacity>
  );

  return (
    <Modal
      visible={visible}
      animationType="slide"
      transparent={true}
    >
      <SafeAreaView style={styles.modalContainer}>
        <View style={styles.header}>
          <Text style={styles.title}>
            Chọn ngôn ngữ {pickerType === 'source' ? 'nguồn' : 'đích'}
          </Text>
          <TouchableOpacity onPress={onClose}>
            <Text style={styles.closeButton}>✕</Text>
          </TouchableOpacity>
        </View>

        {pickerType === 'source' && (
          <TouchableOpacity 
            style={styles.autoDetectButton} 
            onPress={onAutoDetect}
          >
            <Text style={styles.autoDetectText}>🔄 Tự động phát hiện</Text>
          </TouchableOpacity>
        )}

        <FlatList
          data={LANGUAGES}
          renderItem={renderLanguageItem}
          keyExtractor={item => item.code}
        />
      </SafeAreaView>
    </Modal>
  );
};

const styles = StyleSheet.create({
  modalContainer: {
    flex: 1,
    backgroundColor: '#fff',
  },
  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    padding: 16,
    borderBottomWidth: 1,
    borderBottomColor: '#ddd',
  },
  title: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#4B0082',
  },
  closeButton: {
    fontSize: 24,
    color: '#666',
  },
  languageItem: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    padding: 16,
    borderBottomWidth: 1,
    borderBottomColor: '#eee',
  },
  languageText: {
    fontSize: 16,
  },
  checkmark: {
    color: '#4B0082',
    fontSize: 18,
  },
  autoDetectButton: {
    padding: 16,
    borderBottomWidth: 1,
    borderBottomColor: '#eee',
  },
  autoDetectText: {
    fontSize: 16,
    color: '#4B0082',
  },
});
```
Bước 12: Tạo HistoryModal Component
Tạo file src/components/HistoryModal.js:
```sh
import React, { useState } from 'react';
import {
  Modal,
  View,
  Text,
  TouchableOpacity,
  FlatList,
  StyleSheet,
  SafeAreaView
} from 'react-native';
import { LANGUAGES } from '../constants/languages';

export const HistoryModal = ({
  visible,
  history,
  favorites,
  onClose,
  onApplyItem,
  onToggleFavorite
}) => {
  const [showFavorites, setShowFavorites] = useState(false);
  
  const getLanguageName = (code) => {
    return LANGUAGES.find(lang => lang.code === code)?.name || code;
  };

  const renderItem = ({ item }) => (
    <View style={styles.historyItem}>
      <TouchableOpacity 
        style={styles.itemContent}
        onPress={() => onApplyItem(item)}
      >
        <Text style={styles.itemText} numberOfLines={2}>{item.text}</Text>
        <Text style={styles.itemTranslation} numberOfLines={2}>
          {item.translation}
        </Text>
        <Text style={styles.itemLanguages}>
          {getLanguageName(item.sourceLang)} → {getLanguageName(item.targetLang)}
        </Text>
      </TouchableOpacity>

      <TouchableOpacity 
        style={styles.favoriteButton}
        onPress={() => onToggleFavorite(item)}
      >
        <Text style={styles.favoriteIcon}>
          {favorites.some(fav => fav.id === item.id) ? '⭐' : '☆'}
        </Text>
      </TouchableOpacity>
    </View>
  );

  return (
    <Modal visible={visible} animationType="slide">
      <SafeAreaView style={styles.container}>
        <View style={styles.header}>
          <Text style={styles.title}>Lịch sử dịch</Text>
          <TouchableOpacity onPress={onClose}>
            <Text style={styles.closeButton}>✕</Text>
          </TouchableOpacity>
        </View>

        <View style={styles.tabs}>
          <TouchableOpacity 
            style={[styles.tab, !showFavorites && styles.activeTab]}
            onPress={() => setShowFavorites(false)}
          >
            <Text style={styles.tabText}>Tất cả</Text>
          </TouchableOpacity>
          <TouchableOpacity 
            style={[styles.tab, showFavorites && styles.activeTab]}
            onPress={() => setShowFavorites(true)}
          >
            <Text style={styles.tabText}>Yêu thích</Text>
          </TouchableOpacity>
        </View>

        <FlatList
          data={showFavorites ? favorites : history}
          renderItem={renderItem}
          keyExtractor={item => item.id}
        />
      </SafeAreaView>
    </Modal>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
  },
  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    padding: 16,
    borderBottomWidth: 1,
    borderBottomColor: '#ddd',
  },
  // ... thêm các styles khác
});
```
Bước 13: Cập nhật App.js để tích hợp các Modal
Thêm vào App.js:
```sh
import React, { useState, useCallback } from "react";
import {
  View,
  Text,
  TouchableOpacity,
  SafeAreaView,
  StatusBar,
  ActivityIndicator,
  Keyboard,
  Alert,
} from "react-native";
import * as Speech from "expo-speech";
import * as Clipboard from "expo-clipboard";

import { styles } from './src/constants/styles';
import { LANGUAGES } from './src/constants/languages';
import { detectLanguage, translateText } from './src/utils/translation';
import { LanguageSelector } from './src/components/LanguageSelector';
import { TranslationInput } from './src/components/TranslationInput';
import { TranslationResult } from './src/components/TranslationResult';
import { LanguagePicker } from './src/components/LanguagePicker';
import { HistoryModal } from './src/components/HistoryModal';

export default function App() {
  const [text, setText] = useState("");
  const [translatedText, setTranslatedText] = useState("");
  const [sourceLang, setSourceLang] = useState("en");
  const [targetLang, setTargetLang] = useState("vi");
  const [history, setHistory] = useState([]);
  const [favorites, setFavorites] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);
  const [showLanguagePicker, setShowLanguagePicker] = useState(false);
  const [pickerType, setPickerType] = useState("source");
  const [showHistory, setShowHistory] = useState(false);
  const [autoDetect, setAutoDetect] = useState(false);
  const [detectResult, setDetectResult] = useState("");

  const handleDetectLanguage = async () => {
    if (!text.trim()) return;
    
    setIsLoading(true);
    try {
      const detectedCode = await detectLanguage(text);
      if (detectedCode) {
        setSourceLang(detectedCode);
        setDetectResult(`Detected: ${LANGUAGES.find(l => l.code === detectedCode)?.name || detectedCode}`);
      }
    } catch (error) {
      console.error("Language detection error: ", error);
      setError("Could not detect language");
    } finally {
      setIsLoading(false);
    }
  };

  const handleTranslate = async () => {
    if (!text.trim()) return;
    
    Keyboard.dismiss();
    setIsLoading(true);
    setError(null);
    
    try {
      if (autoDetect) {
        await handleDetectLanguage();
      }
      
      const result = await translateText(text, sourceLang, targetLang);
      if (result) {
        setTranslatedText(result);
        
        const newTranslation = {
          id: Date.now().toString(),
          text,
          translation: result,
          sourceLang,
          targetLang,
          timestamp: new Date().toISOString(),
          isFavorite: false
        };
        
        const newHistory = [newTranslation, ...history];
        setHistory(newHistory);
      } else {
        setError("Translation failed");
      }
    } catch (error) {
      console.error("Translation error: ", error);
      setError("Network error. Please try again.");
    } finally {
      setIsLoading(false);
    }
  };

  const swapLanguages = () => {
    if (autoDetect) {
      setAutoDetect(false);
    }
    
    setSourceLang(targetLang);
    setTargetLang(sourceLang);
    setText(translatedText);
    setTranslatedText("");
  };

  const copyToClipboard = async () => {
    if (!translatedText) return;
    
    await Clipboard.setStringAsync(translatedText);
    Alert.alert("Thông báo", "Đã sao chép văn bản!");
  };

  const speakTranslation = () => {
    if (!translatedText) return;
    
    Speech.speak(translatedText, { 
      language: targetLang,
      rate: 0.9,
      pitch: 1.0 
    });
  };

  const toggleFavorite = useCallback((item) => {
    const isFavorite = favorites.some(fav => fav.id === item.id);
    let newFavorites;
    
    if (isFavorite) {
      newFavorites = favorites.filter(fav => fav.id !== item.id);
    } else {
      newFavorites = [...favorites, { ...item, isFavorite: true }];
    }
    
    setFavorites(newFavorites);
  }, [favorites]);

  const applyHistoryItem = (item) => {
    setText(item.text);
    setTranslatedText(item.translation);
    setSourceLang(item.sourceLang);
    setTargetLang(item.targetLang);
    setShowHistory(false);
  };

  const clearText = () => {
    setText("");
    setTranslatedText("");
    setError(null);
  };

  const openLanguagePicker = (type) => {
    setPickerType(type);
    setShowLanguagePicker(true);
  };

  return (
    <SafeAreaView style={styles.container}>
      <StatusBar backgroundColor="#f8f8f8" barStyle="dark-content" />
      
      {/* Header */}
      <View style={styles.header}>
        <Text style={styles.headerTitle}>Quick Translator</Text>
        <TouchableOpacity 
          style={styles.historyButton} 
          onPress={() => setShowHistory(true)}
        >
          <Text style={{fontSize: 24, color: "#4B0082"}}>⏱️</Text>
        </TouchableOpacity>
      </View>
      
      {/* Language Selection */}
      <LanguageSelector
        sourceLang={sourceLang}
        targetLang={targetLang}
        autoDetect={autoDetect}
        onSourcePress={() => openLanguagePicker("source")}
        onTargetPress={() => openLanguagePicker("target")}
        onSwapPress={swapLanguages}
      />
      
      {/* Auto-detect info */}
      {autoDetect && detectResult ? (
        <Text style={styles.detectResult}>{detectResult}</Text>
      ) : null}
      
      {/* Input Area */}
      <TranslationInput
        text={text}
        setText={setText}
        clearText={clearText}
      />
      
      {/* Auto-detect toggle */}
      <TouchableOpacity 
        style={styles.autoDetectToggle}
        onPress={() => {
          setAutoDetect(!autoDetect);
          if (!autoDetect) {
            setDetectResult("");
          }
        }}
      >
        <Text style={{fontSize: 20, color: "#4B0082"}}>
          {autoDetect ? "☑" : "☐"}
        </Text>
        <Text style={styles.autoDetectText}>Tự động phát hiện ngôn ngữ</Text>
      </TouchableOpacity>
      
      {/* Translate Button */}
      <TouchableOpacity 
        style={styles.translateButton} 
        onPress={handleTranslate}
        disabled={isLoading || !text.trim()}
      >
        {isLoading ? (
          <ActivityIndicator size="small" color="#fff" />
        ) : (
          <Text style={styles.translateButtonText}>Dịch</Text>
        )}
      </TouchableOpacity>
      
      {/* Error message */}
      {error ? (
        <Text style={styles.errorText}>{error}</Text>
      ) : null}
      
      {/* Output Area */}
      {translatedText ? (
        <TranslationResult
          translatedText={translatedText}
          onSpeak={speakTranslation}
          onCopy={copyToClipboard}
          onFavorite={() => {
            const currentItem = history[0];
            if (currentItem) {
              toggleFavorite(currentItem);
            }
          }}
          isFavorite={favorites.some(fav => fav.id === history[0]?.id)}
        />
      ) : null}
      
      {/* Language Picker Modal */}
      <LanguagePicker
        visible={showLanguagePicker}
        pickerType={pickerType}
        sourceLang={sourceLang}
        targetLang={targetLang}
        autoDetect={autoDetect}
        onClose={() => setShowLanguagePicker(false)}
        onSelect={(code) => {
          if (pickerType === "source") {
            setSourceLang(code);
            if (autoDetect) setAutoDetect(false);
          } else {
            setTargetLang(code);
          }
          setShowLanguagePicker(false);
        }}
        onAutoDetect={() => {
          setAutoDetect(true);
          setShowLanguagePicker(false);
        }}
      />
      
      {/* History Modal */}
      <HistoryModal
        visible={showHistory}
        history={history}
        favorites={favorites}
        onClose={() => setShowHistory(false)}
        onApplyItem={applyHistoryItem}
        onToggleFavorite={toggleFavorite}
      />
    </SafeAreaView>
  );
}
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

