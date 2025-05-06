
## Table of Contents

- [Introduction to BLoC Pattern](https://claude.ai/chat/83f6c165-dbe2-469c-8d50-3961889bfa48#introduction-to-bloc-pattern)
- [Core Concepts](https://claude.ai/chat/83f6c165-dbe2-469c-8d50-3961889bfa48#core-concepts)
- [Setting Up a BLoC Project](https://claude.ai/chat/83f6c165-dbe2-469c-8d50-3961889bfa48#setting-up-a-bloc-project)
- [Basic BLoC Implementation](https://claude.ai/chat/83f6c165-dbe2-469c-8d50-3961889bfa48#basic-bloc-implementation)
- [Advanced BLoC Techniques](https://claude.ai/chat/83f6c165-dbe2-469c-8d50-3961889bfa48#advanced-bloc-techniques)
- [Testing BLoC](https://claude.ai/chat/83f6c165-dbe2-469c-8d50-3961889bfa48#testing-bloc)
- [BLoC Architecture Patterns](https://claude.ai/chat/83f6c165-dbe2-469c-8d50-3961889bfa48#bloc-architecture-patterns)
- [Real-World Example](https://claude.ai/chat/83f6c165-dbe2-469c-8d50-3961889bfa48#real-world-example)
- [Best Practices](https://claude.ai/chat/83f6c165-dbe2-469c-8d50-3961889bfa48#best-practices)
- [Troubleshooting Common Issues](https://claude.ai/chat/83f6c165-dbe2-469c-8d50-3961889bfa48#troubleshooting-common-issues)
- [Resources](https://claude.ai/chat/83f6c165-dbe2-469c-8d50-3961889bfa48#resources)

## Introduction to BLoC Pattern

BLoC (Business Logic Component) is a state management pattern for Flutter applications originally developed by Google. It helps separate the presentation layer from the business logic, making code more maintainable, testable, and scalable.

### Key advantages of BLoC:

1. **Separation of concerns**: UI components are decoupled from business logic
2. **Testability**: Business logic can be tested independently of the UI
3. **Reusability**: The same BLoC can be used across multiple widgets
4. **Simplified state management**: Provides a consistent pattern for handling state changes
5. **Reactive programming**: Based on Dart Streams for reactive updates

### How BLoC fits in the Flutter ecosystem:

```
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│                 │       │                 │       │                 │
│  Presentation   │ ──────▶     BLoC        │ ──────▶  Data Layer     │
│     Layer       │       │                 │       │                 │
│                 │ ◀──────     (State      │ ◀──────  (Repositories, │
│  (UI Widgets)   │       │   Management)   │       │     Services)   │
│                 │       │                 │       │                 │
└─────────────────┘       └─────────────────┘       └─────────────────┘
       │                         │                         │
       │                         │                         │
       │                         ▼                         │
       │                  ┌─────────────────┐              │
       │                  │                 │              │
       └──────────────────▶      Domain     │ ◀────────────┘
                          │                 │
                          │     (Models,    │
                          │   Use Cases)    │
                          │                 │
                          └─────────────────┘
```

## Core Concepts

### Events

Events are the inputs to a BLoC. They are typically triggered by user interactions or other external events.

```dart
// Events for a counter BLoC
abstract class CounterEvent {}

class IncrementEvent extends CounterEvent {}

class DecrementEvent extends CounterEvent {}

class ResetEvent extends CounterEvent {
  final int resetValue;
  ResetEvent(this.resetValue);
}
```

### States

States represent the output of a BLoC and define what the UI should render.

```dart
// States for a counter BLoC
abstract class CounterState {
  final int counter;
  CounterState(this.counter);
}

class CounterInitial extends CounterState {
  CounterInitial() : super(0);
}

class CounterUpdated extends CounterState {
  CounterUpdated(int counter) : super(counter);
}
```

### BLoC

The BLoC itself manages state transitions based on incoming events.

```dart
// A simple counter BLoC using the bloc package
class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(CounterInitial()) {
    on<IncrementEvent>(_onIncrement);
    on<DecrementEvent>(_onDecrement);
    on<ResetEvent>(_onReset);
  }

  void _onIncrement(IncrementEvent event, Emitter<CounterState> emit) {
    emit(CounterUpdated(state.counter + 1));
  }

  void _onDecrement(DecrementEvent event, Emitter<CounterState> emit) {
    emit(CounterUpdated(state.counter - 1));
  }

  void _onReset(ResetEvent event, Emitter<CounterState> emit) {
    emit(CounterUpdated(event.resetValue));
  }
}
```

## Setting Up a BLoC Project

### Dependencies

Add these dependencies to your `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_bloc: ^8.1.3
  bloc: ^8.1.2
  equatable: ^2.0.5

dev_dependencies:
  bloc_test: ^9.1.4
  mocktail: ^1.0.0
```

### Project Structure

A recommended project structure for a Flutter app using BLoC:

```
lib/
├── blocs/
│   ├── authentication/
│   │   ├── authentication_bloc.dart
│   │   ├── authentication_event.dart
│   │   └── authentication_state.dart
│   └── counter/
│       ├── counter_bloc.dart
│       ├── counter_event.dart
│       └── counter_state.dart
├── data/
│   ├── models/
│   ├── providers/
│   └── repositories/
├── presentation/
│   ├── screens/
│   └── widgets/
├── utils/
├── app.dart
└── main.dart
```

## Basic BLoC Implementation

### Step 1: Define Events and States

First, create the events:

```dart
// counter_event.dart
import 'package:equatable/equatable.dart';

abstract class CounterEvent extends Equatable {
  const CounterEvent();

  @override
  List<Object> get props => [];
}

class IncrementPressed extends CounterEvent {}

class DecrementPressed extends CounterEvent {}
```

Then, create the states:

```dart
// counter_state.dart
import 'package:equatable/equatable.dart';

class CounterState extends Equatable {
  final int count;
  
  const CounterState(this.count);
  
  @override
  List<Object> get props => [count];
}
```

### Step 2: Implement the BLoC

```dart
// counter_bloc.dart
import 'package:flutter_bloc/flutter_bloc.dart';
import 'counter_event.dart';
import 'counter_state.dart';

class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(const CounterState(0)) {
    on<IncrementPressed>(_onIncrementPressed);
    on<DecrementPressed>(_onDecrementPressed);
  }

  void _onIncrementPressed(IncrementPressed event, Emitter<CounterState> emit) {
    emit(CounterState(state.count + 1));
  }

  void _onDecrementPressed(DecrementPressed event, Emitter<CounterState> emit) {
    emit(CounterState(state.count - 1));
  }
}
```

### Step 3: Provide the BLoC to the Widget Tree

```dart
// main.dart
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';
import 'blocs/counter/counter_bloc.dart';
import 'presentation/screens/counter_screen.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter BLoC Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: BlocProvider(
        create: (context) => CounterBloc(),
        child: const CounterScreen(),
      ),
    );
  }
}
```

### Step 4: Consume the BLoC in UI

```dart
// counter_screen.dart
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';
import '../../blocs/counter/counter_bloc.dart';
import '../../blocs/counter/counter_event.dart';
import '../../blocs/counter/counter_state.dart';

class CounterScreen extends StatelessWidget {
  const CounterScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Counter Example'),
      ),
      body: Center(
        child: BlocBuilder<CounterBloc, CounterState>(
          builder: (context, state) {
            return Text(
              '${state.count}',
              style: const TextStyle(fontSize: 48.0),
            );
          },
        ),
      ),
      floatingActionButton: Column(
        mainAxisAlignment: MainAxisAlignment.end,
        crossAxisAlignment: CrossAxisAlignment.end,
        children: [
          FloatingActionButton(
            child: const Icon(Icons.add),
            onPressed: () {
              context.read<CounterBloc>().add(IncrementPressed());
            },
          ),
          const SizedBox(height: 8),
          FloatingActionButton(
            child: const Icon(Icons.remove),
            onPressed: () {
              context.read<CounterBloc>().add(DecrementPressed());
            },
          ),
        ],
      ),
    );
  }
}
```

## Advanced BLoC Techniques

### Cubit - Simplified BLoC

For simpler use cases, you can use Cubit which is a lightweight BLoC that doesn't require explicit events:

```dart
import 'package:flutter_bloc/flutter_bloc.dart';

class CounterCubit extends Cubit<int> {
  CounterCubit() : super(0);

  void increment() => emit(state + 1);
  void decrement() => emit(state - 1);
}
```

Using it in your UI:

```dart
// In your widget
BlocProvider(
  create: (_) => CounterCubit(),
  child: CounterScreen(),
)

// In CounterScreen
BlocBuilder<CounterCubit, int>(
  builder: (context, count) {
    return Text('$count');
  },
)

// Trigger state changes
context.read<CounterCubit>().increment();
```

### BLoC with Repository Pattern

```dart
// user_repository.dart
class UserRepository {
  Future<User> fetchUser(String id) async {
    // API call to fetch user
    return user;
  }
}

// user_bloc.dart
class UserBloc extends Bloc<UserEvent, UserState> {
  final UserRepository userRepository;
  
  UserBloc({required this.userRepository}) : super(UserInitial()) {
    on<FetchUser>(_onFetchUser);
  }
  
  Future<void> _onFetchUser(FetchUser event, Emitter<UserState> emit) async {
    emit(UserLoading());
    try {
      final user = await userRepository.fetchUser(event.userId);
      emit(UserLoaded(user));
    } catch (error) {
      emit(UserError(error.toString()));
    }
  }
}
```

### Handling Side Effects with BLoC

For operations like navigation, showing snackbars, etc., you can use `BlocListener`:

```dart
BlocListener<AuthBloc, AuthState>(
  listener: (context, state) {
    if (state is AuthAuthenticated) {
      Navigator.of(context).pushReplacementNamed('/home');
    } else if (state is AuthError) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text(state.message)),
      );
    }
  },
  child: LoginForm(),
)
```

### Combining Multiple BLoCs

```dart
MultiBlocProvider(
  providers: [
    BlocProvider<AuthBloc>(
      create: (context) => AuthBloc(
        authRepository: RepositoryProvider.of<AuthRepository>(context),
      ),
    ),
    BlocProvider<UserBloc>(
      create: (context) => UserBloc(
        userRepository: RepositoryProvider.of<UserRepository>(context),
      ),
    ),
  ],
  child: MyApp(),
)
```

### BLoC to BLoC Communication

BLoCs can observe and react to changes in other BLoCs:

```dart
class ProfileBloc extends Bloc<ProfileEvent, ProfileState> {
  final AuthBloc _authBloc;
  late final StreamSubscription _authSubscription;
  
  ProfileBloc({required AuthBloc authBloc}) 
      : _authBloc = authBloc,
        super(ProfileInitial()) {
    on<LoadProfile>(_onLoadProfile);
    
    _authSubscription = _authBloc.stream.listen((AuthState authState) {
      if (authState is AuthAuthenticated) {
        add(LoadProfile(userId: authState.user.id));
      } else if (authState is AuthUnauthenticated) {
        emit(ProfileInitial());
      }
    });
  }
  
  @override
  Future<void> close() {
    _authSubscription.cancel();
    return super.close();
  }
  
  Future<void> _onLoadProfile(LoadProfile event, Emitter<ProfileState> emit) async {
    // Implementation
  }
}
```

## Testing BLoC

### Unit Testing a BLoC

```dart
// counter_bloc_test.dart
import 'package:flutter_test/flutter_test.dart';
import 'package:bloc_test/bloc_test.dart';
import 'package:your_app/blocs/counter/counter_bloc.dart';

void main() {
  group('CounterBloc', () {
    late CounterBloc counterBloc;
    
    setUp(() {
      counterBloc = CounterBloc();
    });
    
    tearDown(() {
      counterBloc.close();
    });
    
    test('initial state is 0', () {
      expect(counterBloc.state.count, equals(0));
    });
    
    blocTest<CounterBloc, CounterState>(
      'emits [1] when IncrementPressed is added',
      build: () => counterBloc,
      act: (bloc) => bloc.add(IncrementPressed()),
      expect: () => [const CounterState(1)],
    );
    
    blocTest<CounterBloc, CounterState>(
      'emits [-1] when DecrementPressed is added',
      build: () => counterBloc,
      act: (bloc) => bloc.add(DecrementPressed()),
      expect: () => [const CounterState(-1)],
    );
  });
}
```

### Widget Testing with BLoC

```dart
// counter_page_test.dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:flutter_bloc/flutter_bloc.dart';
import 'package:mocktail/mocktail.dart';
import 'package:your_app/blocs/counter/counter_bloc.dart';
import 'package:your_app/presentation/screens/counter_screen.dart';

class MockCounterBloc extends MockBloc<CounterEvent, CounterState> 
    implements CounterBloc {}

void main() {
  late MockCounterBloc mockCounterBloc;
  
  setUp(() {
    mockCounterBloc = MockCounterBloc();
  });
  
  testWidgets('renders current count', (WidgetTester tester) async {
    when(() => mockCounterBloc.state).thenReturn(const CounterState(42));
    
    await tester.pumpWidget(
      MaterialApp(
        home: BlocProvider.value(
          value: mockCounterBloc,
          child: const CounterScreen(),
        ),
      ),
    );
    
    expect(find.text('42'), findsOneWidget);
  });
  
  testWidgets('calls increment when increment button is pressed',
      (WidgetTester tester) async {
    when(() => mockCounterBloc.state).thenReturn(const CounterState(0));
    
    await tester.pumpWidget(
      MaterialApp(
        home: BlocProvider.value(
          value: mockCounterBloc,
          child: const CounterScreen(),
        ),
      ),
    );
    
    await tester.tap(find.byIcon(Icons.add));
    
    verify(() => mockCounterBloc.add(IncrementPressed())).called(1);
  });
}
```

## BLoC Architecture Patterns

### Clean Architecture with BLoC

```
lib/
├── config/
├── core/
│   ├── error/
│   └── utils/
├── data/
│   ├── datasources/
│   ├── models/
│   └── repositories/
├── domain/
│   ├── entities/
│   ├── repositories/
│   └── usecases/
├── presentation/
│   ├── blocs/
│   ├── pages/
│   └── widgets/
└── main.dart
```

Implementation example:

```dart
// Domain layer
abstract class AuthRepository {
  Future<User> login(String email, String password);
}

class LoginUseCase {
  final AuthRepository repository;
  
  LoginUseCase(this.repository);
  
  Future<User> call(String email, String password) {
    return repository.login(email, password);
  }
}

// Data layer
class AuthRepositoryImpl implements AuthRepository {
  final AuthRemoteDataSource remoteDataSource;
  
  AuthRepositoryImpl(this.remoteDataSource);
  
  @override
  Future<User> login(String email, String password) async {
    final userModel = await remoteDataSource.login(email, password);
    return userModel.toEntity();
  }
}

// Presentation layer
class AuthBloc extends Bloc<AuthEvent, AuthState> {
  final LoginUseCase loginUseCase;
  
  AuthBloc({required this.loginUseCase}) : super(AuthInitial()) {
    on<LoginRequested>(_onLoginRequested);
  }
  
  Future<void> _onLoginRequested(
    LoginRequested event, 
    Emitter<AuthState> emit
  ) async {
    emit(AuthLoading());
    try {
      final user = await loginUseCase(event.email, event.password);
      emit(AuthAuthenticated(user));
    } catch (e) {
      emit(AuthError(e.toString()));
    }
  }
}
```

### Feature-First Organization

Organize code by features rather than technical concerns:

```
lib/
├── core/
├── features/
│   ├── authentication/
│   │   ├── data/
│   │   ├── domain/
│   │   └── presentation/
│   │       ├── bloc/
│   │       ├── pages/
│   │       └── widgets/
│   ├── home/
│   └── profile/
└── main.dart
```

## Real-World Example

Let's build a weather app with BLoC:

### 1. Define Models and Repository

```dart
// weather.dart
class Weather {
  final String city;
  final double temperature;
  final String condition;
  
  Weather({
    required this.city,
    required this.temperature,
    required this.condition,
  });
}

// weather_repository.dart
class WeatherRepository {
  Future<Weather> fetchWeather(String city) async {
    // Simulate API call
    await Future.delayed(const Duration(seconds: 1));
    
    // In a real app, this would be an actual API call
    if (city.toLowerCase() == 'error') {
      throw Exception('Failed to fetch weather data');
    }
    
    return Weather(
      city: city,
      temperature: 20 + (city.length % 10),
      condition: city.length % 2 == 0 ? 'Sunny' : 'Cloudy',
    );
  }
}
```

### 2. Create BLoC

```dart
// weather_event.dart
abstract class WeatherEvent extends Equatable {
  const WeatherEvent();
  
  @override
  List<Object> get props => [];
}

class FetchWeather extends WeatherEvent {
  final String city;
  
  const FetchWeather(this.city);
  
  @override
  List<Object> get props => [city];
}

// weather_state.dart
abstract class WeatherState extends Equatable {
  const WeatherState();
  
  @override
  List<Object> get props => [];
}

class WeatherInitial extends WeatherState {}

class WeatherLoading extends WeatherState {}

class WeatherLoaded extends WeatherState {
  final Weather weather;
  
  const WeatherLoaded(this.weather);
  
  @override
  List<Object> get props => [weather];
}

class WeatherError extends WeatherState {
  final String message;
  
  const WeatherError(this.message);
  
  @override
  List<Object> get props => [message];
}

// weather_bloc.dart
class WeatherBloc extends Bloc<WeatherEvent, WeatherState> {
  final WeatherRepository weatherRepository;
  
  WeatherBloc({required this.weatherRepository}) : super(WeatherInitial()) {
    on<FetchWeather>(_onFetchWeather);
  }
  
  Future<void> _onFetchWeather(
    FetchWeather event, 
    Emitter<WeatherState> emit
  ) async {
    emit(WeatherLoading());
    try {
      final weather = await weatherRepository.fetchWeather(event.city);
      emit(WeatherLoaded(weather));
    } catch (e) {
      emit(WeatherError(e.toString()));
    }
  }
}
```

### 3. Create UI

```dart
// main.dart
void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return RepositoryProvider(
      create: (context) => WeatherRepository(),
      child: BlocProvider(
        create: (context) => WeatherBloc(
          weatherRepository: RepositoryProvider.of<WeatherRepository>(context),
        ),
        child: MaterialApp(
          title: 'Weather App',
          theme: ThemeData(
            primarySwatch: Colors.blue,
          ),
          home: const WeatherScreen(),
        ),
      ),
    );
  }
}

// weather_screen.dart
class WeatherScreen extends StatefulWidget {
  const WeatherScreen({Key? key}) : super(key: key);

  @override
  State<WeatherScreen> createState() => _WeatherScreenState();
}

class _WeatherScreenState extends State<WeatherScreen> {
  final _cityController = TextEditingController();

  @override
  void dispose() {
    _cityController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Weather App'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _cityController,
              decoration: const InputDecoration(
                labelText: 'City',
                hintText: 'Enter city name',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: () {
                if (_cityController.text.isNotEmpty) {
                  context.read<WeatherBloc>().add(
                        FetchWeather(_cityController.text),
                      );
                }
              },
              child: const Text('Get Weather'),
            ),
            const SizedBox(height: 24),
            BlocBuilder<WeatherBloc, WeatherState>(
              builder: (context, state) {
                if (state is WeatherInitial) {
                  return const Center(
                    child: Text('Enter a city to get weather information'),
                  );
                } else if (state is WeatherLoading) {
                  return const Center(child: CircularProgressIndicator());
                } else if (state is WeatherLoaded) {
                  return WeatherCard(weather: state.weather);
                } else if (state is WeatherError) {
                  return Center(
                    child: Text(
                      'Error: ${state.message}',
                      style: const TextStyle(color: Colors.red),
                    ),
                  );
                }
                return const SizedBox.shrink();
              },
            ),
          ],
        ),
      ),
    );
  }
}

// weather_card.dart
class WeatherCard extends StatelessWidget {
  final Weather weather;

  const WeatherCard({Key? key, required this.weather}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Card(
      elevation: 4,
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            Text(
              weather.city,
              style: const TextStyle(
                fontSize: 24,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 8),
            Text(
              '${weather.temperature.toStringAsFixed(1)}°C',
              style: const TextStyle(fontSize: 48),
            ),
            const SizedBox(height: 8),
            Text(
              weather.condition,
              style: const TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 8),
            Icon(
              weather.condition == 'Sunny' 
                  ? Icons.wb_sunny
                  : Icons.cloud,
              size: 64,
              color: weather.condition == 'Sunny' 
                  ? Colors.amber
                  : Colors.blueGrey,
            ),
          ],
        ),
      ),
    );
  }
}
```

## Best Practices

### 1. Keep BLoCs Focused

Each BLoC should have a single responsibility. Instead of having one giant BLoC for your entire application, create smaller, more focused BLoCs.

### 2. Use Equatable for Easy Comparison

Make your event and state classes extend `Equatable` to simplify equality comparisons:

```dart
class LoginState extends Equatable {
  final bool isLoading;
  final String? error;
  final User? user;
  
  const LoginState({
    this.isLoading = false,
    this.error,
    this.user,
  });
  
  @override
  List<Object?> get props => [isLoading, error, user];
}
```

### 3. Leverage Repositories

Keep your BLoCs clean by delegating data operations to repositories:

```dart
class AuthBloc extends Bloc<AuthEvent, AuthState> {
  final AuthRepository _authRepository;
  
  AuthBloc({required AuthRepository authRepository})
      : _authRepository = authRepository,
        super(AuthInitial());
  
  // Event handlers use repository methods
}
```

### 4. Handle Loading States and Errors

Always include loading and error states in your state classes:

```dart
Future<void> _onLoginSubmitted(
  LoginSubmitted event, 
  Emitter<LoginState> emit
) async {
  emit(state.copyWith(isLoading: true, error: null));
  
  try {
    final user = await _authRepository.login(
      event.username, 
      event.password,
    );
    emit(state.copyWith(isLoading: false, user: user));
  } catch (e) {
    emit(state.copyWith(isLoading: false, error: e.toString()));
  }
}
```

### 5. Use BlocObserver for Logging

Register a custom `BlocObserver` to log bloc events for debugging:

```dart
// main.dart
void main() {
  Bloc.observer = SimpleBlocObserver();
  runApp(const MyApp());
}

// simple_bloc_observer.dart
class SimpleBlocObserver extends BlocObserver {
  @override
  void onEvent(Bloc bloc, Object? event) {
    super.onEvent(bloc, event);
    print('${bloc.runtimeType} $event');
  }

  @override
  void onTransition(Bloc bloc, Transition transition) {
    super.onTransition(bloc, transition);
    print('${bloc.runtimeType} $transition');
  }

  @override
  void onError(BlocBase bloc, Object error, StackTrace stackTrace) {
    print('${bloc.runtimeType} $error $stackTrace');
    super.onError(bloc, error, stackTrace);
  }
}
```

### 6. Choose the Right BLoC Widget

- `BlocBuilder`: For rendering UI based on state changes
- `BlocListener`: For side effects (navigation, showing dialogs)
- `BlocConsumer`: Combines builder and listener functionality
- `BlocSelector`: For optimizing rebuilds by selecting specific state properties

### 7. Close BLoCs When Done

Always close your BLoCs when they're no longer needed to avoid memory leaks:

```dart
@override
void dispose() {
  _bloc.close();
  super.dispose();
}
```

## Troubleshooting Common Issues

### 1. State Not Updating

Check if:

- You're using `emit()` in your event handlers
- Your state class correctly implements equality (use Equatable)
- The BLoC is properly provided to the widget tree

### 2. Multiple BLoC Instances

Ensure you're not creating multiple instances of the same BLoC:

```dart
// Incorrect: Creates a new BLoC instance each build
Widget build(BuildContext context) {
  return BlocProvider(
    create: (context) => CounterBloc(),
    child: ChildWidget(),
  );
}

// Correct: Use BlocProvider.value to pass existing BLoC
Widget build(BuildContext context) {
  return BlocProvider.value(
    value: _counterBloc,
    child: ChildWidget(),
  );
}
```

### 3. BLoC Access Issues

If you can't access a BLoC, make sure it's provided higher in the widget tree:

```dart
Widget build(BuildContext context) {
  // This will fail if AuthBloc is not provided in an ancestor widget
  final authBloc = BlocProvider.of<AuthBloc>(context);
  
  // Safer: Returns null if BLoC is not found
  final authBloc = context.read<AuthBloc?>();
}
```

### 4. Infinite Loops

Avoid emitting state in response to state changes:

```dart
// Incorrect: Creates an infinite loop
@override
void onChange(Change<CounterState> change) {
  super.onChange(change);
  if (change.nextState.count > 10) {
    emit(const CounterState(10)); // Don't do this!
  }
}
```

### 5. State Not Persisting

For persistence, combine BLoC with Hydrated BLoC:

```dart
class CounterBloc extends HydratedBloc<CounterEvent, CounterState> {
  CounterBloc() : super(const CounterState(0)) {
    on<IncrementPressed>(_onIncrementPressed);
  }
  
  @override
  CounterState? fromJson(Map<String, dynamic> json) {
    return CounterState(json['count'] as int);
  }
  
  @override
  Map<String, dynamic>? toJson(CounterState state) {
    return {'count': state.count};
  }
}
```

## Resources

- [flutter_bloc Package](https://pub.dev/packages/flutter_bloc