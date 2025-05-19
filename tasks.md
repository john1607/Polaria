# Polaria MVP Build Plan

Each task is small, testable, and focused on a single concern. Tasks are grouped by system component and ordered for logical, test-driven development.

---

## 1. Chrome Extension

### 1.1 Base Setup

- [ ] **1.1.1** Create `manifest.json` with permissions for `activeTab`, `storage`, and `contextMenus`.
- [ ] **1.1.2** Scaffold basic popup HTML with a single "Save" button and minimal UI.
- [ ] **1.1.3** Implement content script to capture selected text on the current page.
- [ ] **1.1.4** Capture page title and URL alongside selected text.

### 1.2 Local Save (Offline First)

- [ ] **1.2.1** Store saved content (text, title, URL, timestamp) in `chrome.storage.local`.
- [ ] **1.2.2** Show visual feedback (e.g. toast or notification) on save.
- [ ] **1.2.3** Add right-click context menu option: "Save selection to Polaria".

### 1.3 Authentication Prep

- [ ] **1.3.1** Add "Sign In" button to extension popup.
- [ ] **1.3.2** Integrate Firebase Auth with Google login.
- [ ] **1.3.3** Use `chrome.identity` to persist user session between extension and web app.

### 1.4 Cloud Sync

- [ ] **1.4.1** On login, sync local saved content to Firestore under the user ID.
- [ ] **1.4.2** Add retry mechanism for failed cloud syncs.

---

## 2. Web Dashboard

### 2.1 Base Setup

- [ ] **2.1.1** Scaffold React app with Tailwind CSS.
- [ ] **2.1.2** Create routes for Dashboard, Login, and Content views.
- [ ] **2.1.3** Integrate Firebase Auth and maintain session across page reloads.
- [ ] **2.1.4** Display logged-in user's email and avatar in header.

### 2.2 Content Viewing

- [ ] **2.2.1** Set up Firestore structure to store user content (`users/{uid}/content`).
- [ ] **2.2.2** Fetch and display saved content in a grid view.
- [ ] **2.2.3** Include title, source domain, date, and excerpt in each card.
- [ ] **2.2.4** Add sorting options (most recent, oldest first).
- [ ] **2.2.5** Implement search by title or keyword.

### 2.3 Content Interaction

- [ ] **2.3.1** Enable full-text view in modal on content click.
- [ ] **2.3.2** Add delete option for each saved content item.
- [ ] **2.3.3** Add editable notes field; sync changes to Firestore.
- [ ] **2.3.4** Add dropdown menu for manual topic tagging.

---

## 3. Question Generation

### 3.1 Backend (Cloud Functions)

- [ ] **3.1.1** Create `/generate-questions` endpoint using Firebase Functions.
- [ ] **3.1.2** Accept text input and return 3 AI-generated comprehension questions via OpenAI or Claude.

### 3.2 Dashboard Integration

- [ ] **3.2.1** Add "Generate Questions" button to each content card.
- [ ] **3.2.2** Send POST request to backend with content body.
- [ ] **3.2.3** Display returned questions inline below content preview.
- [ ] **3.2.4** Store questions in Firestore under corresponding content ID.

---

## 4. Quiz Interface

### 4.1 Quiz Setup

- [ ] **4.1.1** Create "Practice" view with list of tagged topics.
- [ ] **4.1.2** Show available question count per topic.
- [ ] **4.1.3** On topic selection, fetch 3–5 questions for quiz session.

### 4.2 Quiz Experience

- [ ] **4.2.1** Render one question at a time with multiple choice/fill-in-the-blank/true-false types.
- [ ] **4.2.2** Capture and validate user response on submit.
- [ ] **4.2.3** Show correct answer and brief explanation after submission.

### 4.3 Quiz Results

- [ ] **4.3.1** Store user responses and score in Firestore (`users/{uid}/quizzes`).
- [ ] **4.3.2** Show final quiz score and incorrect answers.
- [ ] **4.3.3** Include "Retry Incorrect" option.

---

## 5. Analytics (Basic)

### 5.1 Activity Metrics

- [ ] **5.1.1** Log each quiz session (topic, time, correct/total).
- [ ] **5.1.2** Visualize frequency (bar graph: quizzes per day/week).

### 5.2 Mastery Tracking

- [ ] **5.2.1** Track % correct by topic.
- [ ] **5.2.2** Display per-topic mastery as color-coded progress bar.

---

## 6. Testing Steps (Repeatable After Each Task)

For each testable task above, validate with:

- [ ] **Input Trigger Test:** Manually trigger the action (e.g. click, login, POST).
- [ ] **Data Check Test:** Confirm output in `chrome.storage.local` or Firestore.
- [ ] **UI Feedback Test:** Ensure user receives correct visual cue.
- [ ] **Network Activity Test:** Observe success/failure in dev tools console.

---

## Notes

- Use Firebase Emulator for offline + local dev during early testing.
- Store logs for each step to support rollback and regression testing.
- Allow content saving and viewing to work offline (sync later if unauthenticated).

