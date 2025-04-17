import sys
import os
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QPushButton, QLabel, QLineEdit, QDialog, QFormLayout, QMessageBox, QCheckBox
from PyQt5.QtGui import QPixmap
from PyQt5.QtCore import QTimer

# User class to represent different roles
class User:
    def __init__(self, username, role):
        self.username = username
        self.role = role

# AI Tutor class for animals
class AITutor:
    def __init__(self, name, breed, role, description):
        self.name = name
        self.breed = breed
        self.role = role
        self.description = description

    def interact(self):
        return f"{self.name}, the {self.breed}, is here to help you! {self.description}"

# DogTutor class for AI animal assistants
class DogTutor:
    def __init__(self, name, breed, personality, golden_ring=False):
        self.name = name
        self.breed = breed
        self.personality = personality
        self.golden_ring = golden_ring

    def __str__(self):
        ring_status = "with a golden ring" if self.golden_ring else "without a golden ring"
        return f"{self.name} the {self.breed} ({self.personality}) - {ring_status}"

# Dashboard to track student progress and group projects
class Dashboard:
    def __init__(self):
        self.students = {}
        self.group_projects = {}

    def add_student(self, student_name):
        self.students[student_name] = {
            'tasks_completed': 0,
            'progress': 'Newbie',
            'math_progress': 0,
            'reading_progress': 0,
            'goals': [],
            'messages': []
        }

    def update_student_progress(self, student_name, tasks_completed, math_progress, reading_progress):
        if student_name in self.students:
            self.students[student_name]['tasks_completed'] = tasks_completed
            self.students[student_name]['math_progress'] = math_progress
            self.students[student_name]['reading_progress'] = reading_progress
            if tasks_completed >= 10:
                self.students[student_name]['progress'] = 'Intermediate'
            elif tasks_completed >= 5:
                self.students[student_name]['progress'] = 'Beginner'
            else:
                self.students[student_name]['progress'] = 'Newbie'

    def add_goal(self, student_name, goal):
        if student_name in self.students:
            self.students[student_name]['goals'].append(goal)

    def send_message(self, student_name, message):
        if student_name in self.students:
            self.students[student_name]['messages'].append(message)

    def create_group_project(self, project_name, student_list):
        self.group_projects[project_name] = {
            'students': student_list,
            'chat_log': [],
            'project_data': {}
        }

    def log_group_chat(self, project_name, message):
        if project_name in self.group_projects:
            self.group_projects[project_name]['chat_log'].append(message)

    def display_dashboard(self, role):
        if role in ['teacher', 'parent', 'principal', 'guest']:
            print(f"\n{role.capitalize()} Dashboard:")
            for student, details in self.students.items():
                print(f"Student: {student}")
                print(f"  Overall Progress: {details['progress']}")
                print(f"  Tasks Completed: {details['tasks_completed']}")
                print(f"  Math: {details['math_progress']}%, Reading: {details['reading_progress']}%")
                print(f"  Goals: {details['goals']}")
                print(f"  Messages: {details['messages']}")
                print("---")
        else:
            print("No dashboard available for this role.")

# Splash screen for Play OS
class SplashScreen(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Play OS Boot")
        self.setGeometry(100, 100, 800, 600)
        self.initUI()

    def initUI(self):
        layout = QVBoxLayout()
        self.label = QLabel(self)
        self.label.setText("Welcome to Play OS")
        self.label.setStyleSheet("font-size: 20px; font-weight: bold;")
        layout.addWidget(self.label)

        self.timer = QTimer(self)
        self.timer.timeout.connect(self.closeSplash)
        self.timer.start(3000)  # Auto-close after 3 seconds

        self.setLayout(layout)

    def closeSplash(self):
        self.close()
        self.main_window = MainWindow()
        self.main_window.show()

# Main window for Play OS
class MainWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Play OS - Home")
        self.setGeometry(100, 100, 800, 600)
        self.initUI()

    def initUI(self):
        layout = QVBoxLayout()
        self.label = QLabel("Welcome to Play OS! Choose an option:")
        layout.addWidget(self.label)

        self.start_button = QPushButton("Start Learning")
        self.start_button.clicked.connect(self.startLearning)
        layout.addWidget(self.start_button)

        self.quit_button = QPushButton("Exit Play OS")
        self.quit_button.clicked.connect(self.close)
        layout.addWidget(self.quit_button)

        self.setLayout(layout)

    def startLearning(self):
        QMessageBox.information(self, "Learning", "AI Tutors and learning modules will be added soon!")

# Main execution
if __name__ == "__main__":
    app = QApplication(sys.argv)
    splash = SplashScreen()
    splash.show()
    sys.exit(app.exec_())
