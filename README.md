# Presentation Timer

Simple timer for scientific oral presentations. 

The timer discriminates between following different states:
1. Presentation time (background color: green)
2. Questions/Discussion (background color: orange; warning sound is played)
3. End of allocated time (background color: red; end sound is played)

## Timer Function
- Defining the overall timer duration, a warning time and an alert time before the timer runs out
- The passed time is visualised by a dark radial obverlay
- Timer states have different visual and audio feeback
- Timer states:
    - **green:** normal background during presentation
    - **orange:** warning time (questions)
    - **orange/red flashing:** alert time (close to end of allocated time slot)
    - **red:** allocated time has passed
- All information (incl. sounds) is contained in one html file for ease of operation (no links to external files).
- Start/Stop/Pause/Reset of timer
- Configuration of timer setpoints
- Mute/Unmute of sound output
- Numeric feedback can be changed from displaying only minutes (with fixed :00 for seconds as default) or including seconds
- Numeric feedback scales with screen size
- Control disappear after no user input is detected for clean visuals


## Usage

1. Open html file in a browser
2. Configure the timer
    - **Total Time:** Set the full time
    - **Warning Time:** Set the time remaining when the wrning should appear (color changes and warning sound plays)
    - **Alert Time:** Set the time remaining when the alert should appear (color flashes)
    - **Timer Settigns:** Select whether the time should be shown in minutes only (default) or including seconds.
3. Info provides a brief manual
4. The sound output can be muted
5. Control the timer with Start | Pause | Reset buttons

## Adding new sounds
Any sound files can be added. They must be converted to base64 strings as follows.

### Converting mp3 files
#### Terminal
##### MacOS/Linux
```terminal
base64 -i sound.mp3 -o sound_base64.txt
```

##### Windows (PowerShell) 
```terminal
[Convert]::ToBase64String([IO.File]::ReadAllBytes("warning_chime.mp3")) | Out-File -Encoding ascii sound_base64.txt
```
#### Python
```python
import base64
import os

def generate_html_audio_tag(mp3_filename, tag_id):
    if not os.path.exists(mp3_filename):
        print(f"Error: '{mp3_filename}' not found.")
        return
        
    with open(mp3_filename, "rb") as audio_file:
        # Encode binary data to base64 bytes, then convert to a clean string
        encoded_string = base64.b64encode(audio_file.read()).decode("utf-8")
    
    # Construct the entire HTML tag with the standard audio/mpeg MIME type
    html_tag = f'<audio id="{tag_id}" src="data:audio/mpeg;base64,{encoded_string}"></audio>'
    
    output_filename = f"{tag_id}_tag.txt"
    with open(output_filename, "w", encoding="utf-8") as out_file:
        out_file.write(html_tag)
        
    print(f"Generated tag for {mp3_filename} -> Saved in '{output_filename}'")

# Generate the complete lines for both files
generate_html_audio_tag("warning_chime.mp3", "warningSound")
generate_html_audio_tag("times_up_alarm.mp3", "endSound")
```

substitute the dummy string "//qqqqqqqqqqq" with the obtained base64 string in the html file.

```html
<audio id="warningSound" src="data:audio/mpeg;base64,//qqqqqqqqqqq"></audio>
<audio id="endSound" src="data:audio/mpeg;base64,//qqqqqqqqqqq"></audio>
```

### Sound sources
- https://pixabay.com

## Testing
The timer has been tested with following browsers
- [Firefox](https://www.firefox.com) 151.0.4
- [Chrome](https://www.google.com) 148.0.7778.216
- [Safari](https://www.apple.com/) 26.5