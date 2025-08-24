# Accessible Patient Health Monitoring Platform

Case Study •AWS Solutions Architect, HIPAA, UX Research, Wireframing

# **Executive Summary**

Goal: Design and architect a HIPAA-aligned remote health monitoring platform for visually-impaired and low-vision users that ingests wearable vitals (BP, glucose, SpO₂, HR) and provides accessible, privacy-preserving feedback (voice \+ haptics) while enabling clinicians to act on AI-driven anomaly alerts.

My Role: End-to-end: secondary/desk research, accessibility-first UX, IA & wireframes, high‑fidelity UI spec, AWS architecture, data model, HIPAA mapping, success metrics.

Outcome (simulated): Prototype demonstrates \<2s latency from device → voice feedback, accessible flows pass basic VoiceOver/TalkBack checks, and AI rules surface outliers to providers.

# **Problem & Context**

Visually-impaired and low‑vision users frequently encounter inaccessible health apps (visual graphs, small targets, multi-step flows). Without accessible self-monitoring, issues often go unnoticed, and care teams lack timely data.

## **Constraints**

• Must support eyes‑free interaction (voice \+ haptics, screen readers)  
• PHI must be secured end‑to‑end (HIPAA-aligned)  
• Low cognitive load, simple command model  
• Near real-time feedback for reassurance

# **UX Research & Problem Definition**

Remote Health Monitoring for the Visually Impaired

**UX Research & Problem Definition**

1\. **Secondary Research Insights**

(Desk research compiled from publicly available studies & reports — you can reference WHO, CDC, and American Foundation for the visually-impaired in your case study)

Accessibility Gap in Wearables

Many wearable health apps rely on visual graphs, tiny buttons, and complex navigation, making them 90% less usable for visually-impaired users.  
\[American Foundation for the visually-impaired, 2023\]

Health Condition Overlap  
Over 90% of visually impaired adults over age 50 live with at least one chronic illness (diabetes, hypertension, heart disease).  
\[World Health Organization, 2022\]

Low Adoption of Digital Health Tools  
Only 12% of visually-impaired or low-vision patients regularly track vitals digitally due to poor accessibility features.  
\[CDC, 2021\]

**Preferred Accessibility Features**

**Top three preferences: Voice feedback, haptic (vibration) alerts, and simple one-command navigation**.

**Importance of HIPAA Compliance**  
Visually-impaired users often rely on voice-based interfaces, making the encrypted transmission of spoken PHI (Protected Health Information) crucial to avoid privacy breaches.

**User  Personas**

#### **Persona 1 – Patient (Visually Impaired)**

![](../images/persona%20-%20blind.png)

**Persona 2 – Healthcare Provider**

![](../images/persona%20-%20Nurse.png)

**Empathy Map – David (Visually-impaired Patient)**

![](../images/David%20empathy%20Map.png)

**Problem Statement**

Visually impaired patients lack an accessible, HIPAA-compliant way to monitor vitals from wearables, leading to delayed medical intervention and reduced independence.

1\. **User Journey Map**

![](../images/ChatGPT%20Image%20Aug%2015,%202025,%2002_57_27%20PM.png)

(Scenario: David checks his vitals using a wearable and the app)

| Stage 1 – Trigger Action: David feels dizzy and wants to check his vitals. Touchpoint: Wearable device with one large button. Pain Point: In past apps, no clear audio feedback. Solution: One tap triggers haptic buzz \+ voice prompt: “Checking your vitals now…” | Stage 2 – Data Collection Action: Wearable collects BP, and heart rate. Touchpoint: Sensor \+ AWS IoT Core → Lambda. Pain Point: Long delays in getting readings. Solution: AWS processes data in \<2 seconds, streams it to app.  | Stage 3 – Feedback Action: App announces readings: “Blood pressure 120 over 80\. Heart rate 72.” Touchpoint: Mobile app with Amazon Polly text-to-speech. Pain Point: Users often forget numbers. Solution: App stores encrypted history, retrievable by voice command.  | Stage 4 – Alert Handling Action: AI detects anomaly (e.g., heart rate spike). Touchpoint: AWS Lambda → CloudWatch → SNS sends haptic \+ voice alert: “Your heart rate is unusually high. Contacting your doctor now.” Pain Point: visually-impaired users can’t see push notifications. Solution: Dual feedback: vibration \+ speech. | Stage 5 – Provider Review Action: Data appears in doctor's dashboard. Touchpoint: Encrypted API Gateway → Provider Web App. Pain Point: HIPAA compliance with PHI. Solution: AWS KMS encryption \+ Cognito-secured login. |
| :---- | :---- | :---- | :---- | :---- |

2\. **Low-Fidelity Wireframes**

#### **![](../images/ChatGPT%20Image%20Aug%2015,%202025,%2004_18_27%20PM.png)

| Screen 1 – Home Large central button: “Check My Vitals”. Voice prompt auto-reads menu. Haptic feedback when button is focused. | Screen 2 – Readings Minimal visual clutter; high contrast text. Automatic audio readout: “Blood pressure 120 over 80\. Heart rate 72.” Button: “Repeat Reading” (also voice-activated).  | Screen 3 – Alert Vibration \+ bold text. Voice: “Your oxygen level is low. Calling your provider.” Option: Cancel Alert (voice command).  | Screen 4 – History List view with large spacing. Voice summary for each entry. Filter by date with voice input. |
| :---- | :---- | :---- | :---- |

**High-Fidelity Wireframes**

**![](../images/Screenshot%202025-08-19%20110321.png)

Why Privacy Settings Matter

Because this platform speaks results aloud, there’s a risk of exposing sensitive health data in shared or public spaces. The Privacy Settings page empowers visually-impaired and low-vision users to stay in control of their information:

Privacy Mode → limits spoken output to simple cues like “Vitals normal” or “Vitals alert” instead of full readings.

Auto-Share with Provider → gives patients explicit consent control over when results are transmitted.

Voice PIN → adds an extra layer of protection before results are read aloud.  
This ensures HIPAA alignment, reduces accidental disclosure, and builds user trust — all essential for adoption in a visually-impaired-accessible health app.

Spoken Data Risk:Results are read out loud (via TTS). In public spaces, this could expose private health details.

Privacy Mode toggle → ensures the app only says “Vitals normal” or “Vitals alert” instead of full readings unless the user is in a private space.

Granular Consent  
Some users may want auto-sharing with their care team, others may not.  
Giving the patient explicit toggles supports HIPAA’s principle of patient control.

Security Enhancements  
Option for a voice PIN prevents others from accessing spoken results if the phone is unlocked.

Trust & Adoption  
Visually-impaired users often avoid health apps because they don’t know who hears their data.  
Making privacy transparent and customizable builds trust and adherence.

**3\. AWS HIPAA-Compliant Architecture Diagram**

# **Experience Strategy**

Design Principles:  
• Eyes‑free first (voice, haptics, large targets)  
• One intent per screen (reduce memory load)  
• Predictable feedback (speak what happened; confirm with vibration)  
• Privacy by default (minimize/obfuscate spoken PHI in shared spaces)

**Core Jobs‑to‑be‑Done:**  
• When I initiate a reading, I want a clear spoken confirmation.  
• When results are normal/abnormal, I want an immediate, private cue.  
• When reviewing history, I want quick spoken summaries.

# **User Flows**

Flow A – Quick Check: Launch app → Check vitals → Spoken confirmation.  
Flow B – Alert Handling: If outlier detected → Dual haptic \+ voice alert → Option to notify provider.  
Flow C – History: User requests last 3 readings → Spoken summaries.

# **AWS Architecture (HIPAA‑Aligned)**

![](../images/hipaa-compliant-wearable-device-notifications-system.png)

For a detailed design, click [here](DESIGN.md).

Ingestion: Wearable → AWS IoT Core (mutual TLS)  
Processing: AWS Lambda (validation \+ anomaly checks)  
Storage: DynamoDB (encrypted with KMS)  
APIs: API Gateway with Cognito auth  
Voice: Amazon Polly (TTS)  
Notifications: SNS (voice \+ haptics)

# **Rollout Plan**

• Alpha: Internal accessibility validation with visually-impairedness simulators.  
• Beta: 10–15 visually-impaired/low-vision participants \+ 2 clinicians.  
• General: Documentation, training, support playbooks.

# **Next Steps**

• Phase 2: ML anomaly detection (SageMaker)  
• EHR integration via FHIR APIs  
• Android TalkBack parity \+ multilingual TTS  
• Privacy-preserving analytics
