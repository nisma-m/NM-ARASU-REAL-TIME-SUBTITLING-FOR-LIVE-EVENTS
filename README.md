# NM-ARASU-REAL-TIME-SUBTITLING-FOR-LIVE-EVENTS
Real-time Subtitling for Live Events
Introduction:
Real-time subtitling provides live captioning of audio content, enabling accessibility for
hearing-impaired audiences and improving content engagement. This project simulates a
real-time subtitling system by transcribing pre-recorded audio chunks (.wav files)
sequentially, adding timestamps, and generating subtitle text files.

Problem Statement:
Live events often require real-time subtitles to aid accessibility and comprehension.
Creating an automated system that converts live speech to text with accurate timing is
challenging due to noisy environments, speech variations, and processing delays. This
project addresses the simulation of real-time subtitling using offline audio files to
approximate live transcription.

Objectives:
The objectives of this project are:

● To transcribe audio speech from multiple pre-recorded .wav files sequentially.
● To append recognized text with timestamps to a subtitle file.
● To simulate real-time behavior with controlled delays between audio segments.
● To handle errors gracefully during speech recognition.
● To provide a downloadable subtitle file for user convenience.
Methodology:
The methodology for the real-time subtitling system involves the following steps:

Uploading and Extracting Audio Files
The user uploads a ZIP file containing multiple .wav audio files. These files
represent segmented audio recordings from a live event. The ZIP file is extracted
into a specified directory for processing.
Locating and Sorting Audio Files
The program recursively scans the extracted directory to find all .wav files. These
audio files are then sorted by filename to maintain the correct sequence,
simulating a live audio stream.
Speech Recognition Using Google Web Speech API
For each .wav file, the program uses the Speech Recognition Python library with
Google's Web Speech API to transcribe the speech content into text.
Timestamping and Subtitle Generation
Each recognized text segment is appended to a subtitle file along with a
timestamp indicating the time of transcription. This simulates live subtitle
captions appearing in real-time.
Simulating Real-Time Processing
The system introduces a delay (e.g., 1 second) between processing each audio
segment to imitate real-time subtitling behavior.
Error Handling
The system detects and handles errors such as unrecognized speech or API
failures. It logs appropriate messages and continues processing subsequent audio
files.
Program Flow:
from google.colab import files
import zipfile, os, time
import speech_recognition as sr

Upload and extract ZIP
uploaded = files.upload()
zip_file = list(uploaded.keys())[0]
extract_path = "live_audio"
with zipfile.ZipFile(zip_file, 'r') as zip_ref:
zip_ref.extractall(extract_path)

recognizer = sr.Recognizer()
subtitle_file = "live_subtitles.txt"

Find all wav files
audio_files = []
for root, _, files_in_dir in os.walk(extract_path):
for file in sorted(files_in_dir):

if file.lower().endswith(".wav"):
audio_files.append(os.path.join(root, file))
with open(subtitle_file, "w", encoding="utf-8") as f:
f.write("=== Live Subtitles ===\n")

for filepath in audio_files:
with sr.AudioFile(filepath) as source:
audio = recognizer.record(source)
try:
text = recognizer.recognize_google(audio)
timestamp = time.strftime('%H:%M:%S')
print(f"[{timestamp}] {text}")
with open(subtitle_file, "a", encoding="utf-8") as f:
f.write(f"[{timestamp}] {text}\n")
except sr.UnknownValueError:
print(f"[{time.strftime('%H:%M:%S')}] Could not understand.")
except sr.RequestError as e:
print(f"[{time.strftime('%H:%M:%S')}] API error: {e}")
time.sleep(1)

Input Format:
A ZIP file containing one or more .wav audio files representing audio segments to
be transcribed.

Output Format:
A plain text file named 'live_subtitles.txt' containing timestamped subtitles in the
following format:
[HH:MM:SS] Transcribed speech text

Output:
Testing and Validation:
The system was tested with multiple WAV files of varying lengths and speech clarity.
Recognition accuracy was validated by comparing transcribed text to the original speech.
Error handling was tested by including noisy and silent audio clips.

Conclusion:
This project demonstrated the feasibility of simulating real-time subtitling using
pre-recorded audio files. While not a substitute for true live audio transcription, it
provides a foundation for developing accessible captioning tools. Future work could
include integrating real microphone input, improving recognition accuracy, and adding
advanced subtitle formatting.
