# pi-projects
Dev work on raspberrypi stonemilled projects

Notes on pi issues:
1. wifi dhcp keep failing, switch to static IP on the pi
2. ssh timeouts from vscode to pi. had to turn pi wlan power management to off - set in file to persist withg reboots
3. ordered linux wifi usb to enhance the signal as was low even next to the router



PI IDEAS
Great smoothing of a loop counter display
https://toptechboy.com/ai-on-the-edge-lesson-18-display-frames-per-second-fps-on-opencv-video-window/
fps: float = fps*.95 + (1/fps_loop_delta)*.05 #exponential moving average (EMA)
This is the same idea used in a lot of embedded/robotics telemetry — smoothing sensor noise without needing to buffer a window of samples (no deque, no list, just one running float). It's memory-efficient too, which is nice for a Pi.
