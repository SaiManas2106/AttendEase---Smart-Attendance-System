# AttendEase — Face Recognition Attendance System

> An intelligent, real-time attendance management system powered by facial recognition, built with Python, PyQt5, Dlib, OpenCV, and Firebase.

---

## Table of Contents

- [About the Project](#about-the-project)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation and Setup](#installation-and-setup)
- [Firebase Configuration](#firebase-configuration)
- [Running the Application](#running-the-application)
- [How It Works](#how-it-works)
- [Contributing](#contributing)
- [License](#license)

---

## About the Project

**AttendEase** is a desktop application that automates student attendance using face recognition. Instead of traditional roll-calls or manual entry, AttendEase captures student faces via webcam, identifies them using Dlib's 128D face encoding model, and records attendance directly to a Firebase Realtime Database — all in real time.

It features a clean, modern PyQt5 GUI with a full-screen splash screen, intuitive navigation buttons, and a live date/time display.

---

## Features

- 📷 **Face Registration** — Captures multi-angle face samples (front, left, right, up, down) per student and stores 128D encodings in Firebase
- 🏫 **Start Class** — Selects the subject (DBMS, OS, CS, ASN) and initializes an attendance session
- ✅ **Mark Attendance** — Real-time webcam-based face recognition marks students present automatically
- 📊 **Export Attendance** — Generates and exports full attendance percentage reports to CSV
- 🖥️ **Splash Screen** — Branded startup screen with smooth transition to the main window
- 🕐 **Live Date and Time** — Always-visible real-time clock in the UI

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3.x |
| GUI Framework | PyQt5 |
| Face Detection | Dlib (`get_frontal_face_detector`) |
| Face Landmarks | Dlib (`shape_predictor_68_face_landmarks.dat`) |
| Face Recognition | Dlib ResNet (`dlib_face_recognition_resnet_model_v1.dat`) |
| Video Capture | OpenCV (`cv2`) |
| Database | Firebase Realtime Database |
| Firebase SDK | `firebase-admin` |
| Report Export | Python `csv` module |
| Frontend Assets | HTML, CSS (for report views) |

---

## Project Structure

```
AttendEase/
│
├── main.py                          # Entry point — splash screen + app launch
├── ui_design.py                     # PyQt5 main window UI layout and navigation
├── app.py                           # Core logic: register, attend, export
├── Register_Student.py              # Student registration module
├── Take_Attendance.py               # Attendance marking module
├── Export_Attendance.py             # Attendance export module
│
├── index.html                       # Web-based dashboard (attendance overview)
├── testreports.html                 # Test report viewer
├── style.css                        # Stylesheet for HTML views
│
├── dlib_face_recognition_resnet_model_v1.dat   # Dlib face recognition model
├── shape_predictor_68_face_landmarks.dat        # Dlib landmark model (download required)
│
├── attendease-firebase-adminsdk.json            # Firebase credentials (DO NOT commit)
├── Desktop2.png                     # Background image for UI
└── logo-color.png / logo-color.ico  # App logo assets
```

---

## Prerequisites

Make sure the following are installed on your system:

- Python 3.8 or higher
- pip (Python package manager)
- A working webcam
- A Firebase project with Realtime Database enabled
- CMake (required to build Dlib)

---

## Installation and Setup

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/AttendEase.git
cd AttendEase
```

### 2. Create a Virtual Environment (Recommended)

```bash
python -m venv venv

# Activate on Windows
venv\Scripts\activate

# Activate on macOS/Linux
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install pyqt5 opencv-python dlib numpy firebase-admin
```

> ⚠️ **Dlib Installation Note:** Dlib requires CMake and C++ build tools.
> On Windows, install [Visual Studio Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) before running `pip install dlib`.

### 4. Download the Required Model File

Download the Dlib landmark model and place it in the project root:

- [`shape_predictor_68_face_landmarks.dat`](http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2) — extract the `.bz2` file after downloading

The `dlib_face_recognition_resnet_model_v1.dat` file is already included in the repository.

---

## Firebase Configuration

1. Go to the [Firebase Console](https://console.firebase.google.com/) and create a project
2. Enable **Realtime Database**
3. Go to **Project Settings → Service Accounts → Generate New Private Key**
4. Download the JSON key file and place it in the project root
5. Update `app.py` with your credentials file name and database URL:

```python
cred = credentials.Certificate("your-firebase-adminsdk-key.json")
firebase_admin.initialize_app(cred, {
    'databaseURL': 'https://your-project-default-rtdb.firebaseio.com/'
})
```

> 🔒 **Security:** Add your Firebase credentials JSON to `.gitignore`. **Never push it to GitHub.**

### Recommended `.gitignore`

```
*.json
__pycache__/
*.pyc
venv/
shape_predictor_68_face_landmarks.dat
```

---

## Running the Application

```bash
python main.py
```

The application will:

1. Display the **splash screen** for 3 seconds
2. Launch the **main window** in maximized mode
3. Show the four navigation buttons: Register Student, Start Class, Mark Attendance, Export Attendance

---

## How It Works

### Student Registration

- Enter the student's Roll No. and Name
- The app prompts you to position your face at 5 different angles
- Dlib extracts 128D face encodings from each angle
- Encodings and student data are saved to Firebase

### Taking Attendance

- Select the subject for the current class
- The webcam feed starts and detects faces in real time
- Each detected face is compared against stored encodings using Euclidean distance
- Matched students are marked **Present** in Firebase with a timestamp

### Exporting Attendance

- Aggregates all attendance records from Firebase
- Calculates attendance percentages per student per subject
- Exports a clean, ready-to-use CSV report

---

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a new branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m "Add: your feature description"`
4. Push to the branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

<div align="center">
  <b>Built with ❤️ using Python, Dlib, PyQt5 & Firebase</b>
</div>
