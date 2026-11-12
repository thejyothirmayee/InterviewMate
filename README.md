# InterviewMate 🎯 — Placement OS

A Streamlit placement-preparation platform for students and freshers.

## Main features
- Login with **email OR phone number OR username**
- Resume and Job Description validation (PDF/DOCX/TXT)
- Rejects unrelated files such as bills, receipts, menus, prescriptions, etc.
- Company + job-role specific **Resume Tailor**
- ATS-friendly resume reordering and keyword alignment
- Download tailored resume as TXT or DOCX
- Skill-match and skill-gap analysis
- Personalized preparation roadmap
- Diagnostic test
- Coding, Aptitude, Technical, Project and HR practice
- Full mock placement simulation
- **AI Voice Mock Interview**: spoken two-way interview with browser microphone, speech playback, and automatic submission after 5 seconds of silence
- Progress and readiness dashboard
- SQLite local persistence
- Runs without an API key

## Resume Tailor
Enter the **company**, **job role**, and exact **Job Description**. InterviewMate uses the information already present in your resume to:
1. Create a role-focused professional summary.
2. Put JD-matched skills first.
3. Prioritize relevant existing project/experience lines.
4. Identify missing JD skills/keywords.
5. Produce an ATS-friendly version you can download.

It intentionally does **not invent experience, projects, certifications or skills**. A missing skill should only be added if the candidate genuinely has it.

## AI Voice Mock Interview
Open **🤖 AI Mock Interview** in the sidebar. A saved resume improves personalization but is optional.

- The interviewer speaks questions using your browser's text-to-speech voice.
- Answer aloud through your microphone. After **5 seconds of silence**, the answer is automatically submitted.
- The interviewer reacts and speaks the next question or follow-up. You can repeat a question, pause the microphone, or end the interview for feedback.
- **Complete Placement Interview (All Topics)** includes aptitude, logical reasoning, quantitative reasoning, Python, OOP, DBMS, operating systems, computer networks, coding, project deep-dive, critical thinking, managerial scenarios, communication, HR, workplace judgement and ethics.
- Offline mode uses built-in questions and scoring without an API key. Optional Claude mode needs an `ANTHROPIC_API_KEY` and internet access.
- Voice recognition works best in current Google Chrome or Microsoft Edge. Allow microphone permission. Browser speech recognition availability and network requirements depend on browser/OS.
- Voice is processed by browser speech recognition and speech synthesis; this prototype does not record or save microphone audio. The recognized text is saved in the interview transcript.

Files: `interview_bot.py` (interview logic), `voice_component/index.html` (microphone, 5-second silence detection, and spoken questions). The previous camera proctor component is retained in the project but is not required for voice interviews.


## Responsive interview room and lifecycle safety

- Desktop interview room places the question/conversation panel on the **left** and the live webcam preview on the **right**. On narrow screens, the columns stack for portrait use.
- The voice component has an explicit active/terminal lifecycle. Ending or completing an interview cancels browser speech synthesis, aborts speech recognition, clears silence timers, and prevents delayed callbacks from submitting answers or starting another question.
- Camera monitoring estimates **head direction from face landmarks** in the browser. It does not provide reliable eye-gaze tracking and should not be described as precise eye tracking. A look-away must persist for about 1.2 seconds before a warning; a single sustained turn produces one warning until the candidate returns toward center. A 2.5-second cooldown also separates warning events.
- The warning limit is fixed at **10**. The tenth warning emits a termination event; the Python app saves the reason and final warning count in the existing interview history reason field, then the result view sends a stop command to release the camera.
- Webcam frames are processed in the browser by MediaPipe Face Mesh and are not intentionally uploaded by this app. The browser downloads the MediaPipe library from jsDelivr, so the first load needs internet access.

## Validation notes

Python syntax and embedded JavaScript syntax were checked for this update. A full Streamlit launch and live webcam/microphone browser test were **not run in this environment** because Streamlit is not installed in the execution environment. Run the manual checklist in `TESTING_CHECKLIST.md` after installing the dependencies. Real camera permissions, browser speech recognition, and responsive rendering must be verified in Chrome or Edge on the target device.

## Windows run
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
streamlit run app.py
```

Or double-click `run_interviewmate.bat`.

## Demo positioning
**Resume → Company/Role → JD Match → Resume Tailoring → Skill Gap → Roadmap → Practice → Mock Placement → Readiness**

> Local prototype note: passwords are hashed with SHA-256 for this offline student project. For a production deployment, use a stronger password-hashing scheme such as Argon2/bcrypt and a managed database.


## Expanded interview question dataset (updated)

`question_bank.json` bundles a curated question bank spanning aptitude, logical reasoning, Python, DSA/coding, ML/AI, DBMS/SQL, OS/networks, web/API development, project/resume deep-dives, critical thinking/system design, managerial/HR, and workplace judgement/ethics. Questions are intended to be intermediate to challenging rather than basic definitions. The offline interviewer draws from this bank and prioritises topics based on the saved role, job description and resume text. The bank is original practice material, not a proprietary or official company question dump.

For deeper adaptive interviewing, configure an Anthropic API key in the interview setup or via `ANTHROPIC_API_KEY`. In AI mode, InterviewMate is instructed to analyse the job description and resume, verify claims, ask one question at a time, probe trade-offs and evidence, and adjust follow-ups. Offline mode uses the bundled bank and rule-based follow-ups; it cannot reason as flexibly as a live LLM.
