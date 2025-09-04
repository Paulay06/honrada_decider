import 'package:flutter/material.dart';

void main() {
  runApp(const EmotionTrackerApp());
}

class EmotionTrackerApp extends StatelessWidget {
  const EmotionTrackerApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Emotion Tracker',
      theme: ThemeData(primarySwatch: Colors.purple),
      home: const HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  List<String> moodHistory = [];

  void _selectMood(String mood) {
    setState(() {
      String time =
          "${TimeOfDay.now().hour}:${TimeOfDay.now().minute.toString().padLeft(2, '0')}";
      moodHistory.add("$mood at $time");
    });
  }

  @override
  Widget build(BuildContext context) {
    String today =
        "${DateTime.now().day}/${DateTime.now().month}/${DateTime.now().year}";

    return Scaffold(
      appBar: AppBar(
        title: const Text('Emotion Tracker'),
        actions: [
          IconButton(
            icon: const Icon(Icons.history),
            onPressed: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) =>
                      MoodHistoryPage(moodHistory: moodHistory),
                ),
              );
            },
          )
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              "Hello! How are you feeling today?",
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 5),
            Text("Today: $today",
                style: const TextStyle(fontWeight: FontWeight.w500)),
            const SizedBox(height: 20),
            Wrap(
              spacing: 20,
              runSpacing: 15,
              children: [
                _moodButton("😊 Happy"),
                _moodButton("😢 Sad"),
                _moodButton("😡 Angry"),
                _moodButton("😴 Tired"),
                _moodButton("🤩 Excited"),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _moodButton(String mood) {
    return ElevatedButton(
      onPressed: () => _selectMood(mood),
      style: ElevatedButton.styleFrom(
        padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 15),
        backgroundColor: Colors.purple[300],
      ),
      child: Text(
        mood,
        style: const TextStyle(fontSize: 18),
      ),
    );
  }
}

class MoodHistoryPage extends StatelessWidget {
  final List<String> moodHistory;
  const MoodHistoryPage({super.key, required this.moodHistory});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Mood History")),
      body: moodHistory.isEmpty
          ? const Center(child: Text("No moods tracked yet."))
          : ListView.builder(
              itemCount: moodHistory.length,
              itemBuilder: (context, index) {
                return ListTile(
                  leading: const Icon(Icons.mood),
                  title: Text(moodHistory[index]),
                );
              },
            ),
    );
  }
}
