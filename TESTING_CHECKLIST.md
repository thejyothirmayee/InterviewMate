# InterviewMate validation checklist

Run `pip install -r requirements.txt`, then `streamlit run app.py`. Use a current Google Chrome or Microsoft Edge browser and allow camera/microphone permissions.

## Interview lifecycle and voice
- [ ] Start an interview and confirm the current question is spoken.
- [ ] End the interview while the question is speaking; speech must stop and must not restart.
- [ ] End the interview while microphone recognition is active; recognition must stop.
- [ ] Wait for the result screen and confirm the camera indicator turns off.
- [ ] Confirm no next question or answer submission appears after the interview is terminal.
- [ ] Start a new interview and confirm it starts with fresh audio state and `Warnings 0/10`.

## Camera and warning behavior
- [ ] Confirm the live camera preview is on the right on desktop and questions are on the left.
- [ ] Confirm the mobile layout stacks without horizontal scrolling or overlapping controls.
- [ ] Keep the head turned away for more than approximately 1.2 seconds; confirm one visible “Look at center” warning.
- [ ] Return to center, then look away again after the cooldown; confirm the counter increments once.
- [ ] Confirm the counter cannot exceed `10/10` and the tenth warning terminates the interview and stops audio.
- [ ] Deny camera permission or disconnect the camera; confirm a clear error/termination state appears.
- [ ] Confirm the UI describes this as head-direction estimation, not precise eye-gaze tracking.

## Existing application regression checks
- [ ] Sign in / register.
- [ ] Upload a valid resume and job description; confirm invalid unrelated documents are rejected.
- [ ] Check resume tailoring, job matching, skill gaps, preparation roadmap, practice rounds, dashboard, and interview history.
- [ ] Confirm a completed interview still shows the feedback report and a terminated interview shows the saved reason.

## Notes
The MediaPipe library is loaded from a CDN, so face-landmark monitoring requires internet access at page load. Browser speech recognition support varies; Chrome/Edge are recommended. A text-answer fallback remains part of the surrounding practice app, but the live voice interview currently relies on browser speech recognition.
