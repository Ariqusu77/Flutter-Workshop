-> [Back]

Flutter provides a rich set of UI components (widgets) that help you build beautiful and responsive user interfaces. This guide covers the essential UI components that form the building blocks of any Flutter application.

## Layout Widgets

### Container

The Swiss Army knife of Flutter widgets - provides customization options for padding, margins, borders, background colors, and more.

```dart
Container(
  width: 200,
  height: 200,
  padding: EdgeInsets.all(20),
  margin: EdgeInsets.symmetric(vertical: 10),
  decoration: BoxDecoration(
    color: Colors.blue,
    borderRadius: BorderRadius.circular(10),
    boxShadow: [
      BoxShadow(
        color: Colors.grey.withOpacity(0.5),
        spreadRadius: 5,
        blurRadius: 7,
        offset: Offset(0, 3),
      ),
    ],
  ),
  child: Text('Hello Flutter'),
)
```

### Row and Column

Essential widgets for arranging children horizontally (Row) or vertically (Column).

```dart
// Horizontal arrangement
Row(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  crossAxisAlignment: CrossAxisAlignment.center,
  children: [
    Icon(Icons.star),
    Text('Rating:'),
    Text('4.8'),
  ],
)

// Vertical arrangement
Column(
  mainAxisSize: MainAxisSize.min,
  mainAxisAlignment: MainAxisAlignment.center,
  children: [
    Icon(Icons.phone),
    Text('Call us'),
    Text('123-456-7890'),
  ],
)
```

### Stack

Overlays multiple widgets on top of each other.

```dart
Stack(
  alignment: Alignment.center,
  children: [
    Container(
      width: 300,
      height: 200,
      color: Colors.amber,
    ),
    Text(
      'Stacked Text',
      style: TextStyle(fontSize: 30, color: Colors.black),
    ),
  ],
)
```

### Expanded and Flexible

Control how children resize within Row or Column widgets.

```dart
Row(
  children: [
    // Takes 1/3 of available space
    Expanded(
      flex: 1,
      child: Container(color: Colors.red, height: 100),
    ),
    // Takes 2/3 of available space
    Expanded(
      flex: 2,
      child: Container(color: Colors.blue, height: 100),
    ),
  ],
)
```

## Display Widgets

### Text

Displays styled text.

```dart
Text(
  'Hello Flutter!',
  style: TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.bold,
    color: Colors.purple,
    letterSpacing: 1.5,
    height: 1.5,
  ),
  textAlign: TextAlign.center,
  overflow: TextOverflow.ellipsis,
  maxLines: 2,
)
```

### Image

Displays images from various sources.

```dart
// Network image
Image.network(
  'https://flutter.dev/images/flutter-logo-sharing.png',
  width: 200,
  height: 200,
  fit: BoxFit.cover,
)

// Asset image (from your project)
Image.asset(
  'assets/images/logo.png',
  width: 150,
  height: 150,
)
```

### Icon

Displays vector icons.

```dart
Icon(
  Icons.favorite,
  color: Colors.red,
  size: 36,
)
```

## Input Widgets

### Button Variants

#### ElevatedButton

A material design raised button.

```dart
ElevatedButton(
  onPressed: () {
    print('Button pressed!');
  },
  style: ElevatedButton.styleFrom(
    primary: Colors.blue,
    onPrimary: Colors.white,
    padding: EdgeInsets.symmetric(horizontal: 30, vertical: 15),
  ),
  child: Text('Press Me'),
)
```

#### TextButton

A flat button without elevation.

```dart
TextButton(
  onPressed: () {
    print('Text button pressed!');
  },
  child: Text('Learn More'),
)
```

#### IconButton

A button with just an icon.

```dart
IconButton(
  icon: Icon(Icons.volume_up),
  tooltip: 'Increase volume',
  onPressed: () {
    print('Icon button pressed!');
  },
)
```

#### FloatingActionButton

A circular material design button that floats over content.

```dart
FloatingActionButton(
  onPressed: () {
    print('FAB pressed!');
  },
  backgroundColor: Colors.green,
  child: Icon(Icons.add),
)
```

### TextField

Gets text input from the user.

```dart
TextField(
  decoration: InputDecoration(
    border: OutlineInputBorder(),
    labelText: 'Username',
    hintText: 'Enter your username',
    prefixIcon: Icon(Icons.person),
  ),
  obscureText: false, // Set to true for password fields
  keyboardType: TextInputType.text,
  onChanged: (value) {
    print('Current value: $value');
  },
)
```

### Checkbox and Switch

Toggle widgets for boolean input.

```dart
// Checkbox
Checkbox(
  value: true,
  onChanged: (bool? value) {
    print('Checkbox: $value');
  },
)

// Switch
Switch(
  value: true,
  onChanged: (bool value) {
    print('Switch: $value');
  },
  activeColor: Colors.green,
)
```

## Navigation Widgets

### AppBar

A toolbar at the top of the screen.

```dart
AppBar(
  title: Text('My App'),
  backgroundColor: Colors.blue,
  leading: IconButton(
    icon: Icon(Icons.menu),
    onPressed: () {
      print('Menu button pressed');
    },
  ),
  actions: [
    IconButton(
      icon: Icon(Icons.search),
      onPressed: () {
        print('Search button pressed');
      },
    ),
    IconButton(
      icon: Icon(Icons.more_vert),
      onPressed: () {
        print('More button pressed');
      },
    ),
  ],
)
```

### BottomNavigationBar

A bar at the bottom for switching between views.

```dart
BottomNavigationBar(
  currentIndex: 0,
  onTap: (int index) {
    print('Tapped on item $index');
  },
  items: [
    BottomNavigationBarItem(
      icon: Icon(Icons.home),
      label: 'Home',
    ),
    BottomNavigationBarItem(
      icon: Icon(Icons.search),
      label: 'Search',
    ),
    BottomNavigationBarItem(
      icon: Icon(Icons.person),
      label: 'Profile',
    ),
  ],
)
```

### Drawer

A panel that slides in from the side.

```dart
Drawer(
  child: ListView(
    padding: EdgeInsets.zero,
    children: [
      DrawerHeader(
        decoration: BoxDecoration(
          color: Colors.blue,
        ),
        child: Text(
          'App Menu',
          style: TextStyle(
            color: Colors.white,
            fontSize: 24,
          ),
        ),
      ),
      ListTile(
        leading: Icon(Icons.home),
        title: Text('Home'),
        onTap: () {
          // Navigate to home
        },
      ),
      ListTile(
        leading: Icon(Icons.settings),
        title: Text('Settings'),
        onTap: () {
          // Navigate to settings
        },
      ),
    ],
  ),
)
```

## List Widgets

### ListView

Displays a scrollable list of widgets.

```dart
ListView(
  children: [
    ListTile(
      leading: Icon(Icons.map),
      title: Text('Map'),
    ),
    ListTile(
      leading: Icon(Icons.photo_album),
      title: Text('Album'),
    ),
    ListTile(
      leading: Icon(Icons.phone),
      title: Text('Phone'),
    ),
  ],
)

// ListView.builder for efficient scrolling of many items
ListView.builder(
  itemCount: 100,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text('Item $index'),
    );
  },
)
```

### GridView

Displays items in a grid layout.

```dart
GridView.count(
  crossAxisCount: 2,
  crossAxisSpacing: 10,
  mainAxisSpacing: 10,
  children: List.generate(8, (index) {
    return Container(
      color: Colors.blue[(index + 1) * 100],
      child: Center(
        child: Text(
          'Item $index',
          style: TextStyle(color: Colors.white),
        ),
      ),
    );
  }),
)
```

## Feedback Widgets

### SnackBar

A temporary message at the bottom of the screen.

```dart
// Use with ScaffoldMessenger to show a SnackBar
ScaffoldMessenger.of(context).showSnackBar(
  SnackBar(
    content: Text('This is a snackbar'),
    duration: Duration(seconds: 3),
    action: SnackBarAction(
      label: 'Undo',
      onPressed: () {
        // Code to undo the action
      },
    ),
  ),
);
```

### AlertDialog

A modal dialog for important information.

```dart
showDialog(
  context: context,
  builder: (BuildContext context) {
    return AlertDialog(
      title: Text('Confirm Action'),
      content: Text('Do you want to proceed?'),
      actions: [
        TextButton(
          child: Text('Cancel'),
          onPressed: () {
            Navigator.of(context).pop();
          },
        ),
        TextButton(
          child: Text('OK'),
          onPressed: () {
            // Perform action
            Navigator.of(context).pop();
          },
        ),
      ],
    );
  },
);
```

## Putting It All Together

Here's a simple example combining multiple basic widgets into a functional UI:

```dart
import 'package:flutter/material.dart';

class BasicUIExample extends StatefulWidget {
  @override
  _BasicUIExampleState createState() => _BasicUIExampleState();
}

class _BasicUIExampleState extends State<BasicUIExample> {
  int _selectedIndex = 0;
  bool _switchValue = false;
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Basic UI Example'),
        actions: [
          IconButton(
            icon: Icon(Icons.settings),
            onPressed: () {
              // Open settings
            },
          ),
        ],
      ),
      drawer: Drawer(
        child: ListView(
          padding: EdgeInsets.zero,
          children: [
            DrawerHeader(
              decoration: BoxDecoration(color: Colors.blue),
              child: Text(
                'Navigation Menu',
                style: TextStyle(color: Colors.white, fontSize: 24),
              ),
            ),
            ListTile(
              leading: Icon(Icons.home),
              title: Text('Home'),
              onTap: () {
                Navigator.pop(context);
              },
            ),
            ListTile(
              leading: Icon(Icons.favorite),
              title: Text('Favorites'),
              onTap: () {
                Navigator.pop(context);
              },
            ),
          ],
        ),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Container(
              padding: EdgeInsets.all(20),
              decoration: BoxDecoration(
                color: Colors.amber.shade100,
                borderRadius: BorderRadius.circular(10),
              ),
              child: Text(
                'Welcome to Flutter!',
                style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
              ),
            ),
            SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Text('Enable feature:'),
                SizedBox(width: 10),
                Switch(
                  value: _switchValue,
                  onChanged: (bool value) {
                    setState(() {
                      _switchValue = value;
                    });
                  },
                ),
              ],
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                ScaffoldMessenger.of(context).showSnackBar(
                  SnackBar(content: Text('Button pressed!')),
                );
              },
              child: Text('Show Snackbar'),
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // Add new item
        },
        child: Icon(Icons.add),
      ),
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _selectedIndex,
        onTap: (int index) {
          setState(() {
            _selectedIndex = index;
          });
        },
        items: [
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.business),
            label: 'Business',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.school),
            label: 'School',
          ),
        ],
      ),
    );
  }
}
```

## Additional Resources

- [Flutter Widget Catalog](https://flutter.dev/docs/development/ui/widgets)
- [Material Components Widgets](https://flutter.dev/docs/development/ui/widgets/material)
- [Layout Widgets](https://flutter.dev/docs/development/ui/layout)
- [Flutter Cookbook](https://flutter.dev/docs/cookbook)