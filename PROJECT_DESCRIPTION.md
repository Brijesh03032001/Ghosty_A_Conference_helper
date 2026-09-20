# Ghosty — Project Description

## Frame: Economics — The Transparency Guardrail

## Problem

Professional relationships built at conferences have real career and financial value — a recruiter connection is often cited as being worth $15K–$80K in salary negotiation, and a mentor relationship can shorten a career inflection point by years. Yet this value is invisible, untracked, and often wasted because follow-through fails. By some estimates, 84% of conference connections are never followed up on (HBR). The economic cost of lost social capital is largely hidden.

## Solution

Ghosty is a voice-first mobile conference companion that captures the context behind every human connection before it disappears, and turns social capital into a transparent, actionable career asset.

## Features and Functionality

### Profile & Identity
- Editable profile with bio, career summary, resumes, URLs, skills, and graduation info.
- QR Quick Add for instant identity exchange at events.

### Event Battle Plan
- Pre-event intelligence: attending companies, booth numbers, recruiter names, hiring signals.
- Personalized pitch hints generated for each company based on your skills and career goals.
- Company summary panels with "About", recruiter list, and known contacts count.

### Conversation Capture
- One-tap voice recording via `expo-av` with real microphone permissions on iOS/Android.
- Real OpenAI Whisper transcription when API key is configured.
- Real Claude extraction following `.kiro/steering/extraction-prompt.md` — structured contact cards with name, company, role, intent tag, follow-up date, key details, and uncertainty marking.
- Three capture modes: text note, voice memo, or transcript upload.
- Rule-based review that auto-generates a summary, key points, and action items from the transcript (sentence-splitting and keyword matching in `src/services/ghosty.ts` — not an LLM call).
- Deterministic demo fallbacks when API keys are absent — the app is fully functional for judging without any keys.

### Transparent Connection Value Score (The Transparency Guardrail)
- Every contact receives a score from 1 to 10 with a fully visible breakdown:
  - Role seniority (0–3 points)
  - Company tier (0–2 points)
  - Intent tag weight (recruiting highest, peer lowest)
  - Career relevance (0–2 points based on user's stated goal)
  - Recency bonus (starts at 1, decays 10% per week without follow-up)
- Salary band estimate with source label — never presented as guaranteed compensation.
- Estimated career value derived from median salary band × score.
- This is the core transparency mechanism: a $0 hallway conversation becomes a visible $260K pipeline entry.

### AI-Powered Follow-Up Pipeline
- Prioritized follow-up queue ranked by score × urgency.
- AI-drafted LinkedIn messages, emails, and cover letters that reference the actual captured conversation — not generic templates.
- Three AI quick actions that rewrite drafts in real time:
  - "Make shorter" — Claude condenses the message to 2–3 punchy sentences.
  - "More formal" — Claude rewrites with polished professional tone.
  - "Add skill highlight" — Claude weaves in relevant technical skills naturally.
- Editable drafts with copy-to-clipboard and send actions.

### Magic Wand — Network Intelligence
- Job Search tab: search contacts by job title or company (e.g. "Find software roles").
- Referral Assist tab: pre-built prompts like "Who can introduce me to a hiring manager?" with one-tap search.
- Ranked results with match scores showing why each contact is relevant.

### Social Capital Dashboard
- Estimated career pipeline value (sum of all connection values).
- Social capital growth timeline.
- Intent tag distribution (recruiting, mentor, collaborator, amplifier, peer).
- At-risk connections banner for high-value contacts approaching follow-up decay.
- Event Wrapped summary with top connections and batch follow-up action.

### Privacy by Design
- Explicit tap-to-record only — no ambient listening.
- Red pulsing indicator always visible during recording.
- Supabase schema with row-level security so users own their data.
- Voice memos processed and not stored.

## Competing Signals

### Build Signal
- Full AI pipeline: expo-av recording → Whisper transcription → Claude extraction → transparent scoring → AI draft generation → AI draft refinement.
- Spec-driven development with 6 specs, 4 hooks, and 3 steering docs in `.kiro/`.
- Real API integrations (Whisper + Claude) with deterministic demo fallbacks.
- Supabase schema with RLS policies, plus an edge function for server-side Whisper → Claude processing — designed for real persistence but not yet wired into the running app (which currently stores data in memory).
- TypeScript strict mode, zero type errors across the entire codebase.

### Impact Signal
- Addresses a widely cited problem: an estimated 84% of conference connections are never followed up on (HBR).
- The transparency guardrail makes invisible economic value visible and actionable.
- Real customer journey: student attends event → captures conversations → sees career value → sends personalized follow-ups → converts connections into opportunities.
- Privacy-first design: tap-to-record, no ambient capture, user-owned data.

### Story Signal
- Complete documentation: README, ARCHITECTURE, KIRO_USAGE, DEMO_PITCH.
- Mermaid architecture diagram showing the full system flow.
- `.kiro/` directory with specs, hooks, and steering docs that guided every implementation decision.
- Scalable next steps: browser extension for LinkedIn matching, enterprise CRM integration, aggregated anonymized insight layer.

### Collaboration Signal
- Interdisciplinary team workflow: specs defined requirements before code, steering docs ensured consistency across contributors, hooks documented automated workflows.
- Each team member's domain expertise shaped different parts of the product — from extraction prompt engineering to scoring algorithm design to UI interaction patterns.
- Kiro's spec-driven approach forced explicit decisions that would otherwise remain ambiguous in a 24-hour build.

## Technology Stack

| Layer | Technology |
|---|---|
| Frontend | React Native + Expo |
| Voice capture | expo-av (real mic on iOS/Android) |
| Transcription | OpenAI Whisper API |
| AI extraction & drafting | Claude API (claude-sonnet-4) |
| Scoring | Custom transparent algorithm |
| Backend schema | Supabase Postgres with RLS |
| Edge function | Deno (Supabase Functions) |
| Language | TypeScript (strict mode) |
