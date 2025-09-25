# Lab 02: Navigation in Flutter

## Description

This hands-on lab introduces students to navigation in Flutter by building a multi-page mobile app. Students will learn to create separate screens, navigate between pages, pass data, and implement tabbed navigation using BottomNavigationBar. By the end of this lab, students will have built a personal portfolio app with multiple interconnected screens showcasing Flutter's navigation system.

## Learning Objectives

By completing this lab, students will be able to:

- Create multiple screens as separate Dart files and organize Flutter app structure
- Implement page navigation using Navigator.push() and Navigator.pop() methods
- Pass data between screens using constructor parameters and return values
- Design and implement a BottomNavigationBar with multiple tabs
- Handle navigation state and manage the navigation stack effectively
- Apply named routes for more organized and scalable navigation (challenge task)

## Setup Instructions

### 1. Create a New Flutter Project

Open your terminal/command prompt and run:

```bash
flutter create navigation_portfolio_app
cd navigation_portfolio_app
```

Option 2: Open Android Studio and go to File > New Project > New Flutter Project. Name it lab1. (Skip Step 2)

### 2. Open the Project

Open the project folder in your preferred IDE.

### 3. Run the App

Connect a device/emulator and run:

```bash
flutter run
```

or

In Android Studio press the green icon ▶️ with the chosen device (Emulator of Phisical mobile)

You should see the default Flutter counter app. We'll replace this with our navigation portfolio app.

---

## Lab Tasks

### Task 1: Project Setup and Home Screen

**Objective:** Set up the main app structure and create a home screen with navigation buttons.

**Your Challenge:**
1. Create a folder structure: make a `screens` folder inside `lib`
2. Set up `main.dart` to use a `HomeScreen` as the home widget
3. Create the home screen with welcome content and navigation buttons

**Starter Structure for `main.dart`:**
```dart
import 'package:flutter/material.dart';
import 'screens/home_screen.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Portfolio App',
      theme: ThemeData(
        primarySwatch: Colors.indigo,
        // TODO: Add visual density and other theme properties
      ),
      home: HomeScreen(),
    );
  }
}
```

**Home Screen Requirements:**
- Create `lib/screens/home_screen.dart`
- Include an AppBar with title "My Portfolio"
- Add a welcome card with:
  - CircleAvatar with person icon
  - Welcome text and subtitle
- Add an "Explore My Work" section
- Create placeholder navigation buttons (we'll make them functional in Task 2)

**Hints:**
- Use `StatelessWidget` for the home screen
- Structure with `Column` and `Card` widgets
- Navigation buttons should be full-width using `SizedBox(width: double.infinity)`
- Buttons should have icons: `Icons.person` for About, `Icons.work` for Projects

### Task 2: Create Additional Screens and Basic Navigation 

**Objective:** Create About and Projects screens, then implement Navigator.push() and Navigator.pop().

**Your Challenge:**
1. Create `AboutScreen` and `ProjectsScreen` as separate files
2. Add actual navigation functionality to the home screen buttons
3. Test the navigation flow with back button functionality

**AboutScreen Structure:**
```dart
// lib/screens/about_screen.dart
import 'package:flutter/material.dart';

class AboutScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('About Me'),
        // TODO: Note that back button is automatic
      ),
      body: Padding(
        padding: EdgeInsets.all(20),
        child: Column(
          children: [
            // TODO: Add Card with personal information
            // TODO: Add contact info with Row widgets (email, location)
            // TODO: Add custom back button (optional - Flutter provides one)
          ],
        ),
      ),
    );
  }
}
```

**ProjectsScreen Requirements:**
- Create a list of 3 sample projects using `List<Map<String, String>>`
- Use `ListView.builder` to display projects as Cards
- Each project should have: title, description, tech stack
- Add `ListTile` with leading icon, title, subtitle, and trailing arrow
- Add `onTap` for now just use `print('Tapped on ${project['title']}')`

**Navigation Implementation:**
Update your home screen buttons to use:
```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => AboutScreen()),
);
```

**Hints:**
- Import your new screen files in `home_screen.dart`
- `Navigator.pop(context)` for custom back buttons
- Use `isThreeLine: true` in ListTile for better project card layout

### Task 3: Passing Data Between Screens 

**Objective:** Create a project detail screen that receives data and returns values.

**Your Challenge:**
1. Create `ProjectDetailScreen` with constructor parameters
2. Navigate from projects list with data
3. Return data back to the calling screen

**ProjectDetailScreen Constructor:**
```dart
// lib/screens/project_detail_screen.dart
class ProjectDetailScreen extends StatelessWidget {
  final String title;
  final String description;  
  final String tech;

  ProjectDetailScreen({
    required this.title,
    required this.description,
    required this.tech,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
        actions: [
          // TODO: Add favorite button that returns 'favorited' when pressed
        ],
      ),
      body: Padding(
        padding: EdgeInsets.all(20),
        child: Column(
          children: [
            // TODO: Display project information in Cards
            // TODO: Add action buttons (Share, Mark as Viewed) that return different values
          ],
        ),
      ),
    );
  }
}
```

**Navigation with Data:**
In your `projects_screen.dart`, replace the onTap with:
```dart
onTap: () async {
  final result = await Navigator.push(
    context,
    MaterialPageRoute(
      builder: (context) => ProjectDetailScreen(
        // TODO: Pass the project data here
      ),
    ),
  );
  
  // TODO: Handle the returned result with a SnackBar
},
```

**Hints:**
- Use `Navigator.pop(context, 'some_value')` to return data
- `ScaffoldMessenger.of(context).showSnackBar()` for showing results
- Action buttons can return different strings: 'shared', 'viewed', 'favorited'

### Task 4: Bottom Navigation Bar with Multiple Tabs

**Objective:** Create a main navigation screen with BottomNavigationBar to switch between sections.

**Your Challenge:**
1. Create a `ContactScreen` with contact information and a simple form
2. Create `MainNavigationScreen` that handles bottom navigation without state management
3. Update main.dart to use the new navigation structure

**ContactScreen Requirements:**
- Contact information as ListTiles (email, phone, LinkedIn)
- Simple message form with TextField widgets
- Send button that shows a demo SnackBar

**MainNavigationScreen Structure:**
```dart
// lib/screens/main_navigation_screen.dart
class MainNavigationScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return DefaultTabController(
      length: 4,
      child: Scaffold(
        body: TabBarView(
          children: [
            // TODO: Add your four screens here
          ],
        ),
        bottomNavigationBar: Container(
          decoration: BoxDecoration(
            border: Border(top: BorderSide(color: Colors.grey[300]!, width: 1)),
          ),
          child: TabBar(
            tabs: [
              // TODO: Create tabs with icons and labels
              // Icons: Icons.home, Icons.work, Icons.person, Icons.contact_mail
            ],
            labelColor: Colors.indigo,
            unselectedLabelColor: Colors.grey,
            indicatorColor: Colors.indigo,
            indicatorWeight: 3,
          ),
        ),
      ),
    );
  }
}
```

**Contact Form Structure:**
```dart
Column(
  children: [
    TextField(
      decoration: InputDecoration(
        labelText: 'Your Name',
        border: OutlineInputBorder(),
      ),
    ),
    // TODO: Add message TextField with maxLines: 3
    // TODO: Add send button
  ],
)
```

**Hints:**
- Choose either the `TabController` approach (recommended) or the `Navigator.pushReplacement` approach
- `DefaultTabController` manages tab state automatically without StatefulWidget
- For the second approach, each tab creates a new instance of MainNavigationScreen
- Update main.dart to use `MainNavigationScreen()` as home

### Task 5: Challenge - Named Routes 

**Objective:** Implement named routes for the project detail navigation (optional advanced task).

**Your Challenge:**
Refactor the project detail navigation to use named routes instead of direct MaterialPageRoute.

**Routes Setup in main.dart:**
```dart
MaterialApp(
  // ... existing properties
  routes: {
    '/home': (context) => MainNavigationScreen(),
    '/project-details': (context) {
      // TODO: Extract arguments and create ProjectDetailScreen
      // Hint: Use ModalRoute.of(context)!.settings.arguments
    },
  },
)
```

**Navigation Usage:**
Replace the Navigator.push in projects screen with:
```dart
Navigator.pushNamed(
  context,
  '/project-details',
  arguments: {
    // TODO: Pass project data as Map<String, String>
  },
);
```

**Hints:**
- Arguments are passed as `Map<String, String>`
- Extract arguments with: `final args = ModalRoute.of(context)!.settings.arguments as Map<String, String>;`
- Access values with: `args['title']!`, `args['description']!`, etc.

---

## Deliverables

You are required to .zip file this project and deliver it in Moodle before the next class. 
The .zip should contain all the content of inside project folder with the exception of the **.dart_tool** and **build** folders (these will make the .zip file extremely large and they are not necessary for evaluation).

## Tips and Additional Resources

### Navigation Best Practices
- Always provide clear visual indicators for navigation (back buttons, tab highlights)
- Consider user expectations for back button behavior on different platforms
- Use consistent navigation patterns throughout your app

### Common Navigation Issues
- Remember to import screen files in files that navigate to them
- Use `await` when you need to handle return values from navigation
- `setState()` is required for updating BottomNavigationBar current index
- Arguments in named routes must be cast to the correct type

