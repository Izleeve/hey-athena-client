# Athena Voice
Your personal robotic assistant.

## Usage Examples
"Athena"
*(double beep)*
"Turn up"
*(plays music)*

"Athena"
*(double beep)*
"What is the price of bitcoin right now?"
*(responds with the current bitcoin price)*

## Core Dependencies
- Python 3
- Pocketsphinx
    - Sphinxbase (packaged with pocketsphinx)
- SpeechRecognition
    - https://pypi.python.org/pypi/SpeechRecognition/#downloads
- Pyglet
    - https://bitbucket.org/pyglet/pyglet/wiki/Download
    - AVBin (library file must be seen by pyglet)
        http://avbin.github.io/AVbin/Download.html
- PyAudio
    - Linux/Mac: https://people.csail.mit.edu/hubert/pyaudio/
    - Unofficial Windows: http://www.lfd.uci.edu/~gohlke/pythonlibs/#pyaudio
- gTTS
    - requests (packaged with gTTS)

## Windows Installation (Python 3.4)
- Currently, pocketsphinx does not seem to install on python 3.5
- Users not using Python 3.4 and above might need to install `pip` command tool
- Download unofficial PyAudio:
    - For Python 3.4 users, download `PyAudio‑0.2.8‑cp34‑none‑win32.whl`  (tip: cp34 = Python 3.4)
    - http://www.lfd.uci.edu/~gohlke/pythonlibs/#pyaudio
- Open command prompt and switch to the download directory:
    - `cd (download directory)`
    - `pip3 install PyAudio‑0.2.8‑cp34‑none‑win32.whl`
    - `pip3 install pocketsphinx SpeechRecognition pyglet gTTS wolframalpha`
- Download the `athena-voice-client` repository and extract it
- Add `C:\path\to\athena-voice-client-master` to your `PYTHONPATH` system or user environment variable
    - One easy way to do this is to import the project into Eclipse (PyDev) and have it add the project to PYTHONPATH
- `cd athena-voice-client-master\client`
- If all goes well, run `brain.py`, say "Athena", and ask her a question!

## Active Modules
Active modules contain a series of tasks. Each task parses user input (generally through regex) and, if it matches, responds accordingly.

### Active Module Example
```python
from client.classes.module import Module
from client.classes.task import ActiveTask
from client.tts import play_mp3

class PlaySongTask(ActiveTask):
    def __init__(self):
        # Give regex input patterns to the task
        # This line is not required, but is generally helpful
        super().__init__(patterns=[r'.*(\b)+turn(\s)up(\b)+.*'])
         
    def match(self, text):
        # Use the given input patterns to match STT (text) input
        for p in self.patterns:
            if p.match(text):
                return True
        return False
    
    def action(self, text):
        # Turn up
        self.speak("Turning up...")
        play_mp3("turnup.mp3") # Searches "/media" folder if no path given
        
class Music(Module):
    def __init__(self):
        # Add tasks and basic info to the module
        tasks = [PlaySongTask()]
        super().__init__(mod_name='music', mod_tasks=tasks, mod_priority=2)
```

## Passive Modules
(not yet implemented)
Passive modules will be scheduled tasks run in the background. Future versions may have event triggers for modules as well.
