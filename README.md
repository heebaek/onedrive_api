# 📂 OneDriveApi

A Dart package that provides convenient access to the OneDrive API, built on top of `oauth2restclient`.

---

## ✨ Features

- 🔐 OAuth2 authentication via `oauth2restclient`
- 📄 List, upload, download, copy, move, and delete OneDrive files
- 🗂 List and create folders
- 💡 Easy access to OneDrive Streams and metadata
- 📁 Supports both personal and business OneDrive
- 🛤 Path-based API for intuitive file operations

---

## 📦 Installation

Add to your `pubspec.yaml`:

```yaml
dependencies:
  one_drive_api: ^0.0.1
```

---

## 🚀 Getting Started

```dart
import 'package:one_drive_api/one_drive_api.dart';

void main() async {
  final account = OAuth2Account();

  // OneDrive용 OAuth2 provider 등록
  account.addProvider(OneDrive(
    clientId: "YOUR_CLIENT_ID",
    redirectUri: "YOUR_REDIRECT_URI",
    scopes: [
      "User.Read",
      "Files.ReadWrite.All",
      "Files.Read.All",
      "openid",
      "email",
      "offline_access",
    ],
  ));

  // 로그인 또는 토큰 로드
  final token = await account.newLogin("onedrive");
  final client = await account.createClient(token);

  // API 인스턴스 생성
  final onedrive = OneDriveRestApi(client);

  // 드라이브 정보 가져오기
  final drive = await onedrive.getDrive();
  print("Drive ID: ${drive.id}");

  // 루트 폴더 파일 목록 조회
  final items = await onedrive.listChildren("/");

  for (final item in items.value) {
    print("${item.name} (${item.id})");
  }
}
```

---

## 📂 Example Operations

- **List Files**:
```dart
// 루트 디렉토리 내용 조회
await onedrive.listChildren("/", top: 20);

// 특정 폴더 내용 조회
await onedrive.listChildren("/Documents", top: 20);
```

- **Upload File**:
```dart
// 루트에 파일 업로드
await onedrive.upload("/example.txt", fileStream);

// 특정 폴더에 파일 업로드
await onedrive.upload("/Documents/report.pdf", fileStream);
```

- **Create Folder**:
```dart
// 루트에 폴더 생성
await onedrive.createFolder("/", 'New Folder');

// 특정 폴더 안에 하위 폴더 생성
await onedrive.createFolder("/Documents", 'Work');
```

- **Download File**:
```dart
// 파일 다운로드
final stream = await onedrive.download("/Documents/file.txt");
```

- **Copy File**:
```dart
// 파일 복사
await onedrive.copy("/Documents/file.txt", "/Backup");
```

- **Move/Rename File**:
```dart
// 파일 이동
await onedrive.move("/Documents/file.txt", "/Pictures");

// 파일 이동하면서 이름 변경
await onedrive.move("/Documents/file.txt", "/Pictures", "new_name.txt");
```

- **Delete File**:
```dart
// 파일 삭제
await onedrive.delete("/Documents/file.txt");

// 폴더 삭제
await onedrive.delete("/Documents/Old Folder");
```

---

## �� Path-based API

이 라이브러리는 직관적인 경로 기반 API를 제공합니다:

- **절대 경로**: `/`로 시작하는 경로 (예: `/Documents/file.txt`)
- **루트 디렉토리**: `/`는 OneDrive의 루트 디렉토리를 나타냅니다
- **폴더 경로**: `/Documents`, `/Pictures` 등
- **파일 경로**: `/Documents/report.pdf`, `/Pictures/photo.jpg` 등

### 경로 예제

```dart
// 루트 디렉토리
"/"

// 폴더
"/Documents"
"/Pictures"
"/Documents/Work"

// 파일
"/Documents/file.txt"
"/Pictures/photo.jpg"
"/Documents/Work/report.pdf"
```

---

## 🔗 Dependencies

- [`oauth2restclient`](https://pub.dev/packages/oauth2restclient)

---

## 📄 License

MIT License © Heebaek Choi 