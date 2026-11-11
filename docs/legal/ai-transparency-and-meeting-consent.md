> **DRAFT - NOT LEGAL ADVICE - NOT FOR PUBLICATION.** Prepared 2026-11-11 by Hannah Weiss (security-architect, AI team member) for review and finalisation by qualified counsel. Bracketed items `[LIKE THIS]` are open decisions; see `README.md` checklist.

# Tavieri AI Transparency and Meeting Consent Notice

Parts: **A** user-facing notice, **B** meeting-participant notice, **C** texts the bot posts and says, **D** product rules that make the wording true. Product behaviour in D is a requirement on engineering; if it cannot be met, the wording must change.

## A. Notice for Tavieri users (shown at sign-up and in Settings)

**You are interacting with an AI.** Tavieri's assistant, voice and avatar are artificial intelligence, not a person. The avatar is a stock or synthetic character; it is not a real person's likeness. Audio, images and video it produces are labelled as AI-generated and carry machine-readable marking where technically supported.

**What the AI can do.** Join meetings you invite it to, listen, transcribe, summarise, propose actions, speak if you enable it, and use tools you connect. It never acts outside the permissions you grant, and actions with external effect need your approval under your policy.

**What it does not do.** It does not identify people by voice or face, does not build voiceprints, does not infer emotions, and does not secretly join or record.

**Your responsibilities.** Before inviting the AI, you must have the right to do so and tell participants. In many places (for example many US states, the EU and the UK) recording or transcribing needs the consent of everyone, or a lawful basis plus notice. The consent tools below help you; **they do not replace your own obligation.** Do not use the AI where you cannot meet it.

**Your controls.** Pause or stop capture instantly (visible state indicator and a Stop button), remove the AI from the meeting, delete recordings and transcripts, and keep audio off (default).

**Errors.** AI can be wrong. Review summaries and proposed actions before relying on them.

## B. Notice for meeting participants (invite text, join page and link)

> **An AI notetaker from Tavieri may join this meeting.**
> [HOST NAME] has invited **Tavieri AI Assistant**, an artificial intelligence, to this meeting. It will [transcribe the conversation and the chat, and create a summary and action items] for [HOST NAME / ORGANISATION]. It is not a person. It does not identify anyone by voice or face and does not analyse emotions. [It will appear with the name "Tavieri AI Assistant (AI)".]
>
> **Your choice.** You can ask the host to remove it, or tell the meeting you do not agree, at any time. If anyone objects, capture stops. You may also leave the meeting.
>
> **Your data.** [HOST ORGANISATION] controls the transcript and decides how long it is kept; Tavieri processes it on their behalf. Contact [HOST ORGANISATION PRIVACY CONTACT] or **[DPO or PRIVACY CONTACT EMAIL]** to exercise rights such as access or deletion. Privacy information: [PRIVACY POLICY URL].

## C. Texts the bot posts or says

**Bot display name:** `Tavieri AI Assistant (AI)` (never a human-like name; never changeable by the host to hide AI status).

**C1. Chat message on joining (posted before any capture):**
> I am Tavieri AI Assistant, an artificial intelligence (not a person), invited by [HOST NAME] to [transcribe this meeting and write a summary]. Nothing is captured until the host has confirmed below. If you do not agree, say so or type "STOP" in this chat and I will stop capturing immediately. The host can remove me at any time. Notice and your rights: [SHORT URL]

**C2. Spoken announcement (voice, on joining; repeated for late joiners):**
> "Hello. This is Tavieri AI Assistant, an AI, not a person. [Host name] invited me to transcribe this meeting and make a summary. If anyone does not agree, say 'stop recording' or type STOP in the chat, and I will stop right away."

**C3. Late-joiner chat message (each time a new participant joins):**
> Reminder: Tavieri AI Assistant (AI) is transcribing this meeting for [HOST NAME]. Type STOP to stop it.

**C4. Capture-start confirmation (after the policy in D1 is satisfied):**
> Transcription has started. Type STOP at any time to end it.

**C5. On an objection or STOP:**
> Understood. Capture has stopped. [Anything captured so far will be deleted / is marked for deletion unless the host keeps it where lawful.] The host can restart it only after re-confirming consent.

**C6. End of meeting:**
> The meeting ended. Summary and transcript go to [HOST NAME]. Questions or deletion requests: [SHORT URL].

**C7. If the AI speaks or presents an avatar:** a persistent on-screen "AI" label, and a spoken disclosure at first utterance: "I am an AI assistant."

## D. Product rules the wording depends on

1. **Consent gate (default "strict"):** capture starts only when (a) the host or meeting owner approves the bot (and the platform's own approval prompt is accepted), (b) the host has affirmed in-product that they have the right and have informed participants, and (c) the join announcements C1 and C2 were delivered. In meetings where any participant's location is unknown, treat as **all-party**: allow [10-30] seconds for objections after C2 and ensure objectors can stop capture before it begins. [COUNSEL: choose between this "notice plus no objection" model and a stricter explicit opt-in (e.g. every participant types "I agree"), per jurisdiction. Express consent is safer under all-party-consent statutes and Quebec Law 25; implied consent by staying is contested.]
2. **Any participant can stop it.** "STOP" in chat or by voice (best-effort speech detection) pauses capture within seconds; resumption requires the host to re-confirm. Removal by the host ends capture immediately.
3. **Visible state.** The bot is visible in the participant list with its AI name; platform "recording" indicators are used where available (Zoom RTMS disclosure, Teams "Unverified" label, Google Meet indicators). No hidden or headless capture.
4. **Late joiners** receive C3 and (if speaking is enabled) C2.
5. **Minimal retention.** Raw audio off by default; transcripts retained per Privacy Policy; deletion on request honoured, including for participants who objected.
6. **No biometrics.** No speaker voiceprint enrolment; speaker labels come from the platform's participant metadata only. No face recognition or emotion inference.
7. **AI Act Art. 50.** Disclosure of AI interaction occurs at or before first interaction (C1/C2/C7); synthetic audio and avatar video are marked in machine-readable form; avatars are stock or synthetic (not a real person), which avoids the deepfake labelling duty for deployers but not the disclosure duty.
8. **Evidence.** Store a consent/disclosure record per meeting (who approved, what was posted, timestamps, objections) for [X] years.
9. **Language and locale.** Texts are localised (including French for Quebec). [COUNSEL: translations require legal review.]
10. **Do not rely on screenshots of the user's own "agree" in Terms** as meeting-participant consent: participants are third parties.

## E. Open points for counsel
All-party-consent state statutes (CA, FL, IL, MD, MA, MT, NH, PA, WA, DE and NV are commonly listed; sources disagree and statutes were not verified), whether strict opt-in is required, GDPR basis for non-user participants, ePrivacy/telecom secrecy rules in Germany and Austria (DACH is the launch market) and employee-works-council issues, Quebec and PIPEDA express consent, UK, and the Otter.ai litigation (N.D. Cal.) lessons.
