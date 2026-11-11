# 03 - Real-time media, voice AI and meeting participation: build vs buy

Author: realtime/voice research role. Sources accessed 2026-11-10/11. "UNVERIFIED" = not confirmed on an official page. Prices are USD list prices, subject to change.

## 1. Key findings (sourced)

### 1.1 Meeting-bot infrastructure (Zoom / Meet / Teams)

**Recall.ai can speak and show a camera feed in all three platforms.** "Output Media" makes the bot run a webpage you control and stream its audio and video into the meeting as camera (or screenshare). Zoom, Meet, Teams and Webex are listed. Video is 1280x720 at 15 fps, audio 16 kHz mono, added audio latency under 50 ms per Recall's FAQ (vendor claim). The page can use `getUserMedia` to hear meeting audio, open a WebSocket for transcripts, and pipe audio to a speech-to-speech model. Limits: the camera cannot be turned off (placeholder image is the workaround), no audio-only mode, bot video is not in recordings, not combinable with the plain output-audio/video endpoints. Sources: https://docs.recall.ai/docs/stream-media
**Recall price:** $0.50/h pay-as-you-go, prorated per second, 5 h free, startup program $0.25/h for first 10k h; `web_4_core` +$0.10/h (total $0.60/h), `web_gpu` +$1/h (total $1.50/h); built-in transcription $0.15/h. https://www.recall.ai/pricing

| Vendor | Platforms | Outbound audio/video | Price/h | Self-host / licence |
|---|---|---|---|---|
| Recall.ai | Zoom, Meet, Teams, Webex, GoTo, Slack | Yes, audio and webpage-video (above) | 0.50 (+0.10 4-core) | No (hosted) |
| Attendee | Zoom, Meet, Teams (Webex per docs) | Docs: "bots can speak arbitrary audio", webcam images, voice-agent webpage streaming via camera/screenshare | 0.50 falling to 0.35; 5 h free; self-host = enterprise plan | Self-host yes, but licence is **Elastic License 2.0** (source-available; forbids offering the software to third parties as a hosted service). 796 stars (UNVERIFIED current) |
| MeetingBaaS | Zoom, Meet, Teams | Speaking bots (audio, STT/LLM/TTS pipeline); avatar/video not mentioned | from 0.35 plus ~0.45-0.5 token for speaking bot | SDK/speaking-bot repos MIT; core API self-host UNVERIFIED |
| Vexa | Meet, Teams, Zoom | Open-core README: `/speak` returns 404 in v0.12 (so no speaking); third-party listing claims speak (UNVERIFIED) | hosted ~0.30 | Apache-2.0, ~3k stars, Docker/Helm |
| Nylas Notetaker, MeetStream, Skribby | Zoom/Meet/Teams | Output not verified (UNVERIFIED) | ~0.35 | no |

Sources: https://docs.attendee.dev/ , https://www.attendee.dev/pricing , https://raw.githubusercontent.com/attendee-labs/attendee/main/LICENSE , https://www.meetingbaas.com/pricing , https://github.com/Vexa-ai/vexa , https://github.com/Meeting-BaaS , https://www.nylas.com/blog/best-meeting-bot-apis/

**Reliability:** no independent data found. Recall's claim that a rival scored 42% on its 300-call test is a competitor statement. A third-party engineering write-up says self-hosted Attendee/Vexa is "as reliable as your ops team" and browser-automation bots break when platforms change their UI; self-host needs about 4 vCPU per bot (~$0.40/h compute) and the build-vs-buy crossover is in the hundreds of thousands of hours/month. https://www.forasoft.com/blog/article/meeting-bot-api-architecture (vendor-adjacent, treat as directional).

**Official platform re-verification:**
- **Google Meet Media API**: verified. "Developer Preview Program is discontinued and will not proceed to GA"; signups closed; endpoints shut down **15 January 2027**; no replacement named. https://developers.google.com/workspace/meet
- **Teams real-time media**: verified. Application-hosted media bots need .NET and Windows Server; the page says "Real-time Media bots are not recommended for AI agent scenarios" (points to Copilot Studio agents and Graph transcripts); intended for CVI, compliance recording, contact centre; "significant infrastructure investment". Page updated 2026-05-29. The platform does support sending audio and video. https://learn.microsoft.com/en-us/microsoftteams/platform/bots/calls-and-meetings/real-time-media-concepts
- **Teams external bots (new, not in the docs)**: Microsoft Message Center MC1251206 says Teams detects external meeting-assistant bots, labels them in the lobby ("Unverified" per a trade report), and by default requires organizer approval; admin policy can block; GA rolling out through end of October 2026. https://mc.merill.net/message/MC1251206 , https://uctoday.com/ai-meeting-bots-controls-microsoft-zoom-google
- **Zoom RTMS**: verified inbound only (audio PCM 16 kHz, video JPG/PNG or H.264, screen share, transcript, chat); the docs describe no way to send media back. Needs credits; forum answer from a Recall employee says 0.01 credit/min (0.02 with transcription), credit price not stated (UNVERIFIED). https://developers.zoom.us/docs/rtms/ , https://developers.zoom.us/docs/rtms/meetings/media/ , https://devforum.zoom.us/t/rtms-credit-consumption-per-minute-and-whether-the-initiating-participant-needs-a-paid-plan/145391
- **Zoom Meeting SDK**: quote verified on the Linux SDK page: "designed for human use cases and does not support bots or AI notetakers". Secondary sources add OBF/ZAK/RTMS authorisation required for joining outside own account from 2 March 2026. https://godevelopers.zoom.us/docs/meeting-sdk/linux

**Does third-party infra make "Meet/Teams infeasible" wrong?** Wrong as a feasibility statement, right as a statement about official APIs. Recall/Attendee join as ordinary participants and do two-way audio and video on all three platforms. What changes is the risk class: dependence on a vendor that automates a client, platform bot-policy (Teams lobby approval, Zoom enterprise blocking of third-party bots, Google admin controls UNVERIFIED), and per-hour cost. Meeting join is therefore "feasible but permission-gated", not "infeasible".

### 1.2 Frameworks
- **LiveKit Agents (Python)**: Apache-2.0, ~15.2k stars, PyPI 1.8.11 (2026-11-05), Python 3.10-3.14. Turn detection: semantic turn-detector model (LiveKit Model License, free on LiveKit Cloud), VAD, STT-endpointing, manual; adaptive interruption (filters backchannel "mm-hmm") and resume-after-false-interruption. 16 avatar plugins (Anam, bitHuman, D-ID, LiveAvatar, Simli, Tavus, Synthesia, Runway and others): avatar worker joins the room, agent sends audio to it, it publishes synced A/V. SIP/telephony built in. Cloud: Build $0 (1k session min), Ship $50 (5k then $0.01/min), Scale $500 (50k, 600 concurrent); telephony ~$0.01/min. https://github.com/livekit/agents , https://docs.livekit.io/agents/models/avatar/ , https://docs.livekit.io/agents/build/turns/ , https://livekit.com/pricing
- **Pipecat**: BSD-2, ~17.2k stars, PyPI 1.15.0 (2026-10-29), Python >=3.11. Transports: Daily, LiveKit, SmallWebRTC (self-hosted P2P), WebSocket, Twilio/SIP. Avatar services: Tavus, HeyGen, Simli, LemonSlice. Smart Turn v3: open weights, BSD-2, 23 languages, ~8M params, runs locally on ONNX. Pipecat Cloud $0.01/min active (agent-1x), Daily voice+video $0.004/participant-min, SIP $0.003-0.02/min. https://github.com/pipecat-ai/pipecat , https://github.com/pipecat-ai/smart-turn , https://www.daily.co/pricing/pipecat-cloud/ . Meeting BaaS's own speaking bot is built on Pipecat.

### 1.3 Speech/LLM stack (verified list prices)
| Component | Price |
|---|---|
| Deepgram Nova-3 streaming STT | $0.0077/min direct; $0.0042-0.0058 via LiveKit Inference; Flux $0.0065-0.0078 |
| Cartesia Sonic TTS | 1 credit/char, ~750-800 chars/min audio; $37.5-50 per 1M chars via LiveKit; 44 languages |
| ElevenLabs Flash TTS | $0.04/1k chars (~$0.04/min), ~75 ms model latency, 32 languages; v3 70+ langs $0.08 |
| Deepgram Voice Agent (all-in) | $0.075/min |
| OpenAI gpt-realtime-2.1 audio | $32 in / $64 out per 1M tokens; mini $10 / $20 |
| OpenAI GPT-Live-1 | **$0.05/min flat** front-end voice layer, backend model billed separately; launched 2026-09-10 |
| Gemini 3.8 Live | audio in $3/1M (~$0.005/min), out $12/1M (~$0.018/min); 25 audio tokens/s |
| LLM for cascade | GPT-5.4 mini $0.75/$4.50 per 1M; GPT-5 mini $0.25/$2; Gemini 3.1 Flash Lite $0.25/$1.50 |

Sources: https://livekit.com/pricing/inference , https://developers.openai.com/api/docs/pricing , https://aistudio.google.com/docs/pricing , https://deepgram.com/pricing , https://cartesia.ai/pricing , https://elevenlabs.io/pricing/api . Note LiveKit-reseller and direct prices differ; use the direct price as the high case.

**Latency (end of user turn to first audio):** no independent apples-to-apples benchmark found. Vendor/secondary ranges: cascaded 600-1,200 ms, speech-to-speech 300-700 ms; one secondary test measured 0.90 s (OpenAI realtime) and 1.14 s (Google). GPT-Live-1 turn latency 0.8 s is a secondary figure (UNVERIFIED; OpenAI page returned 403). A vendor blog claims S2S agents degrade under noise/accents (UNVERIFIED design). Plan to measure P50/P95 ourselves. https://coval.ai/blog/voice-ai-models-2026/ , https://www.dograh.com/feeds/blog/speech-speech-vs-cascade
**Languages:** Deepgram Nova 60+, ElevenLabs v3 70+, Cartesia 44, Smart Turn 23; GPT-Live-1 and Gemini Live language lists UNVERIFIED.

### 1.4 Live avatars (audio in, video out)
| Vendor | Price / min | Notes |
|---|---|---|
| Simli | $0.01 (Hobby), $0.009 (Scale), enterprise to $0.005 | custom avatars per plan; LiveKit + Pipecat integrations; latency UNVERIFIED |
| bitHuman | $0.02 self-hosted/on-device, $0.04 cloud | runs on CPU/edge, offline licence $899/character |
| HeyGen LiveAvatar | LITE 1 credit (~$0.09), FULL 2 credits (~$0.18) | LITE = you supply audio; LiveKit/Pipecat plugins |
| Anam | $0.11-0.16 plan overage; enterprise from $0.04 | commercial use needs paid plan |
| Tavus CVI | $0.26-0.35 overage | full pipeline (STT/LLM/TTS/perception) bundled; 5-60 min conversation caps |
| D-ID | pricing page gave no numbers (UNVERIFIED) | LiveKit plugin exists |

Sources: https://www.simli.com/pricing , https://www.bithuman.ai/pricing , https://docs.liveavatar.com/docs/faq/credits , https://anam.ai/pricing , https://www.tavus.io/pricing . No avatar latency figures were verifiable.
**Self-host:** MuseTalk 1.5 (MIT code, commercial use allowed): "30fps+ on a V100", mouth/face region 256x256, some jitter and lost facial detail; LivePortrait is not lip-sync. Diffusion-based streaming research (SoulX-FlashTalk 14B, 32 fps, 0.87 s start) has no verified weights/licence/GPU count. https://github.com/TMElyralab/MuseTalk , https://arxiv.org/abs/2512.23379 . GPU rental (RunPod secure): L4 $0.59/h, RTX 4090 $0.89/h, L40S $1.09/h, H100 $2.89-3.99/h. https://www.runpod.io/pricing . Streams per GPU: UNVERIFIED (assumption 1-4).

## 2. Numbers: fully-loaded cost per session minute (USD)
Assumptions: cascaded stack (Deepgram STT + GPT-5.4-mini-class LLM + Cartesia/ElevenLabs TTS), agent speaks ~50% of a 1:1 minute; infra = LiveKit/Pipecat Cloud agent minute (~$0.01; low case self-hosted ~$0.003). Avatar billed for whole connected minute (conservative). Meeting adds Recall $0.0083/min ($0.50/h), typical $0.01 (4-core), high $0.025 (GPU). Self-hosted avatar: GPU $/h / streams / utilisation (low 4 streams L40S at 80%; typical 2 streams 4090 at 40%; high 1 stream L40S at 30%). Excludes engineering time, tool-call LLM cost, egress, storage, payment fees. The 1:1 voice-only low/typical (0.023/0.038) brackets LiveKit's own $0.048 phone-agent calculator (includes $0.01 telephony and $0.01 observability).

| Mode | 1:1 low | 1:1 typical | 1:1 high | Meeting low | Meeting typical | Meeting high |
|---|---|---|---|---|---|---|
| Voice-only (cascaded) | 0.023 | 0.038 | 0.13 | 0.031 | 0.048 | 0.155 |
| Voice-only (Gemini Live S2S) | 0.024 | 0.03 | 0.06 | 0.032 | 0.04 | 0.085 |
| Voice + vendor avatar | 0.033 (Simli) | 0.13 (~$0.09 avatar) | 0.40 (Tavus bundle) | 0.041 | 0.14 | 0.425 |
| Voice + self-hosted avatar | 0.028 | 0.058 | 0.19 | 0.036 | 0.068 | 0.215 |

Reading it: typical voice-only user spending 60 min/month costs about $2.3; with a Simli-class avatar about $3; with a mid-priced avatar about $8. Avatar vendor choice swings cost 10x; self-hosted GPU only beats a $0.01/min vendor if utilisation is high, so it is not worth building before volume. Gemini-S2S figures are my arithmetic from verified token prices plus infra; OpenAI realtime token-per-minute rates are UNVERIFIED so GPT-Live-1's flat $0.05 + backend model (~$0.06-0.09 total) is the comparable.

## 3. Risks
- Meeting platforms may block or label bots (Teams lobby approval default; Zoom admin blocks); Zoom's own policy says SDK is not for bots; legal standing of third-party Zoom bots UNVERIFIED.
- Single-vendor dependence (Recall) and Recall output media needs a 4-core bot; extra hop adds latency to the avatar path (end-to-end UNVERIFIED).
- Attendee's ELv2 licence may restrict using it inside a hosted multi-tenant product: needs legal read.
- Avatar vendors' latency, uptime and concurrency limits unverified; conversation caps on lower plans.
- Voice cloning/face likeness consent and AI-disclosure/recording laws (owner/legal).
- Prices moved a lot in 2026 (Recall -29%, Cartesia agent halved); re-quote before budgets.

## 4. Recommendation
**Buy the meeting layer, build the agent core, abstract everything else.**
- **Phase 1 (own app):** web WebRTC session on **LiveKit Agents** (Python), cascaded STT (Deepgram) + LLM (router) + TTS (Cartesia), semantic turn detector, adaptive interruption. Start on LiveKit Cloud Ship ($50) and keep the SFU self-hostable (Apache-2.0). Voice first; then add avatar through the plugin with **Simli** (cheapest verified, ~$0.01/min) and bitHuman as second vendor; keep Gemini Live / GPT-Live-1 as an optional S2S path behind the same interface for low latency premium tiers. Do not build GPU avatar hosting yet.
- **Phase 2 (meetings):** **Recall.ai Output Media**. The bot's webpage is Tavien's own "stage" page (TypeScript) that joins the same LiveKit room as a subscriber, plays agent audio and avatar video, and forwards meeting audio to the room. One agent backend serves 1:1 and meetings. Disclose AI in bot name and chat; admit via lobby. Fallback: **Attendee** self-hosted (after licence review) or MeetingBaaS; Zoom RTMS (inbound, cheap) as a later cost optimisation. Teams real-time-media bot and Meet Media API: do not build.
- Runner-up framework: **Pipecat** (BSD, self-hosted SmallWebRTC for the cheapest 1:1, Smart Turn); keep adapters so we can swap in a day-scale spike.
- Build line: orchestration, policy/consent, adapters, usage metering, stage page. Buy: SFU (initially), STT/TTS/LLM, avatar render, meeting join.

**Thin Python adapter interface (names only):** `RoomTransport`, `RealtimeSession`, `SpeechToText`, `TextToSpeech`, `SpeechToSpeechModel`, `LlmClient`, `TurnDetector`, `AvatarProvider`, `AvatarSession`, `MeetingProvider`, `MeetingEvent`, `CapabilityProfile`, `TelephonyGateway`, `UsageMeter`, `DisclosurePolicy`.

## 5. Where the initial docs are wrong, outdated or optimistic
- Meet shutdown date (15 Jan 2027), Teams "not recommended" and Zoom Meeting-SDK quotes are accurate.
- "Meet/Teams infeasible" and "Zoom outbound unproven" ignore browser/SDK bot vendors that already provide two-way audio and video on all three; the real gate is platform bot policy, not capability.
- Omitted: Teams external-bot labelling and organizer approval (GA by Oct 2026), Zoom OBF authorisation (March 2026), per-hour costs ($0.35-0.65/h).
- Avatar spec assumes low-latency lip-sync as a given; vendors publish no latency numbers, and open-source MuseTalk is 256x256 with jitter.
- No unit economics were in the docs; avatar is the dominant cost driver.

## 6. Open questions for the owner
1. Approve a Recall.ai account (pay-as-you-go, 5 free hours) and a LiveKit Cloud/Simli trial for spikes?
2. Monthly budget ceiling per user for voice/avatar minutes (drives plan caps)?
3. Legal sign-off: bot ToS on Zoom/Meet/Teams, Attendee ELv2, recording-consent and AI-disclosure wording, likeness/voice-cloning policy.
4. Is a stock avatar acceptable for launch, or is a custom brand face required?
