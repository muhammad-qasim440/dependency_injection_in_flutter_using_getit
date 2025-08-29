# Dependency Injection in Flutter using GetIt

![Flutter CI](https://github.com/muhammad-qasim440/dependency_injection_in_flutter_using_getit/actions/workflows/flutter_test.yml/badge.svg)

A Flutter project demonstrating **Dependency Injection (DI)** with the [GetIt](https://pub.dev/packages/get_it) service locator.  
This repository is designed to help learners and developers understand how to decouple Flutter code for **clean architecture**, **testability**, and **maintainability**.

---

## 🚀 Features
- Setup and configure **GetIt** in Flutter
- Register services, repositories, and view models
- Property injection & method injection examples
- Decouple UI from business logic
- Mock dependencies for testing

---

## 📂 Project Structure
lib/
│-- main.dart # App entry point
│-- core/ # Core setup (DI, config)
│ └── service_locator.dart # GetIt registrations
│-- services/ # Example services
│-- repositories/ # Example repositories
│-- viewmodels/ # Example ViewModels
│-- ui/ # Flutter UI widgets
test/
│-- user_repository_test.dart # Unit tests
.github/
│-- workflows/
└── flutter_test.yml # GitHub Actions workflow


---

## 🛠️ Getting Started

### Prerequisites
- Flutter SDK (>=3.0.0)
- Dart (>=3.0.0)

### Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/muhammad-qasim440/dependency_injection_in_flutter_using_getit.git
   cd dependency_injection_in_flutter_using_getit
Install dependencies:
flutter pub get
Run the app:
flutter run

📖 Example Code
1. Setup GetIt (service_locator.dart)
import 'package:get_it/get_it.dart';
import '../services/api_service.dart';
import '../repositories/user_repository.dart';

final getIt = GetIt.instance;

void setupLocator() {
  // Service layer
  getIt.registerLazySingleton<ApiService>(() => ApiServiceImpl());

  // Repository layer
  getIt.registerFactory<UserRepository>(
    () => UserRepositoryImpl(getIt<ApiService>()),
  );
}

2. Use Dependency in a ViewModel
import '../repositories/user_repository.dart';
import '../core/service_locator.dart';

class UserViewModel {
  final UserRepository _userRepository = getIt<UserRepository>();

  Future<void> loadUsers() async {
    final users = await _userRepository.fetchUsers();
    print(users);
  }
}

3. Inject into UI
import 'package:flutter/material.dart';
import '../viewmodels/user_viewmodel.dart';

class HomeScreen extends StatelessWidget {
  final UserViewModel viewModel = UserViewModel();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("DI with GetIt")),
      body: Center(
        child: ElevatedButton(
          onPressed: () => viewModel.loadUsers(),
          child: Text("Fetch Users"),
        ),
      ),
    );
  }
}

🧪 Testing Example

With DI, you can replace services with mocks/fakes in tests:

import 'package:flutter_test/flutter_test.dart';
import 'package:get_it/get_it.dart';
import 'package:dependency_injection_in_flutter_using_getit/services/api_service.dart';
import 'package:dependency_injection_in_flutter_using_getit/repositories/user_repository.dart';

// Mock Service
class MockApiService implements ApiService {
  @override
  Future<List<String>> fetchUsers() async {
    return ["MockUser1", "MockUser2"];
  }
}

void main() {
  final getIt = GetIt.instance;

  setUp(() {
    getIt.reset();
    getIt.registerSingleton<ApiService>(MockApiService());
    getIt.registerFactory<UserRepository>(
      () => UserRepositoryImpl(getIt<ApiService>()),
    );
  });

  test('UserRepository fetchUsers returns mock data', () async {
    final repo = getIt<UserRepository>();
    final users = await repo.fetchUsers();

    expect(users, isNotEmpty);
    expect(users, contains("MockUser1"));
    expect(users.length, 2);
  });
}

📚 Resources

GetIt package

Flutter Official Docs

Effective Dart Guidelines

🏷️ Tags

flutter · dart · dependency-injection · getit · clean-architecture · flutter-examples · service-locator

📌 License

This project is licensed under the MIT License - see the LICENSE
 file for details.


---

✅ Once you push your repo with the `.github/workflows/flutter_test.yml` file, the badge will automatically update.  

