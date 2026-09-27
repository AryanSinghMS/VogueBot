# Software Requirements Specification (SRS)
## Project Name: VogueBot — AI-Powered Personal Styling Assistant
**Academic Level:** Bachelor of Computer Applications (BCA) Project  
**Version:** 1.0.0  
**Status:** Approved / Active Baseline  
**Target Environment:** Web Browser (Desktop & Mobile Responsive)

---

## 1. Introduction

### 1.1 Purpose
The purpose of this document is to define the functional, non-functional, architectural, and operational requirements for **VogueBot**, an intelligent personal fashion and styling assistant web application. This document serves as the single source of truth for design, development, evaluation, and viva/project presentations.

### 1.2 Project Scope
VogueBot addresses the daily challenge of outfit selection and aesthetic discovery. The system acts as a personal digital stylist through a conversational chat interface where users describe occasions, aesthetic desires, weather, or style dilemmas, and receive:
1. Context-aware conversational styling advice.
2. Structured visual outfit recommendation cards (imagery, aesthetic tags, outfit composition, item descriptions).
3. Preset style exploration (Retro 70s, Streetwear, Modern Minimalist, Dark Academia, Y2K, Formal/Office, etc.).
4. Session tracking and personalized style preferences.

### 1.3 Project Philosophy: "Grounded & Feasible"
To ensure guaranteed completion, reliable live demonstration, and zero unnecessary infrastructure cost for a student-level BCA project:
- **No Complex Heavyweight Infrastructure:** Avoid multi-server microservices or paid cloud GPU clusters.
- **Budget-Friendly / Free-Tier AI:** Utilize accessible APIs (Google Gemini API free tier / rule-based fallback) with minimal latency.
- **Modern Lightweight Web Stack:** Built on React 19 and Vite for fast client-side rendering with instant local execution.
- **Persistence:** LocalStorage for immediate demo stability, with a clean path for an optional lightweight Express/SQLite backend.

### 1.4 Definitions & Acronyms
- **SRS:** Software Requirements Specification
- **UI/UX:** User Interface / User Experience
- **SPA:** Single Page Application
- **LLM:** Large Language Model (e.g., Google Gemini Flash)
- **API:** Application Programming Interface
- **JSON:** JavaScript Object Notation
- **HMR:** Hot Module Replacement (Vite)

---

## 2. Overall Description

### 2.1 Product Perspective
VogueBot is a standalone, client-centric web application with optional backend connectivity for AI prompt processing and catalog storage.

```
+----------------------------------------------------------------+
|                        Client Browser                          |
|  +--------------------+  +------------------+  +-------------+ |
|  |  Sidebar / History |  | Chat Interaction |  | Outfit Card | |
|  +--------------------+  +------------------+  +-------------+ |
|                              |                                 |
|                       React State Store                        |
|                     (LocalStorage Cached)                      |
+----------------------------------------------------------------+
                               |
               +---------------+---------------+
               |                               |
       [Direct API / Backend]          [Curated Asset]
       Google Gemini 1.5 Flash          Static Fashion 
        (or Fallback Engine)            Catalog & CDN
```

### 2.2 User Classes & Personas
1. **General User / Student / Working Professional:** Wants quick, reliable outfit recommendations based on specific events (interview, date, campus fest, casual brunch).
2. **Fashion Enthusiast:** Wants to explore specific aesthetic genres (e.g., Y2K, 70s retro, cottagecore) and understand color harmonies.
3. **Project Evaluator / Examiner:** Tests prompt edge-cases, inspects code modularity, UI responsiveness, and architectural clarity.

### 2.3 Operating Environment
- **Client Platforms:** Modern web browsers (Google Chrome 110+, Mozilla Firefox 110+, Apple Safari 16+, Microsoft Edge).
- **Display Resolutions:** Desktop (1920x1080, 1440x900, 1366x768), Tablet (768x1024), Mobile (375x667 to 430x932).
- **Runtime Environment:** Node.js (v18.x or v20.x LTS) with Vite development server.

### 2.4 Design & Implementation Constraints
1. **Tooling Limitation:** Restricted to well-documented, standard web technologies (React, JavaScript, CSS3, Vite, Node).
2. **Cost Constraint:** Must operate at $0 recurring infrastructure cost (free-tier APIs, client-side caching, GitHub Pages/Vercel/Render deployment).
3. **Offline / Fallback Resilience:** In case of API rate limits or lack of internet connectivity, the application must gracefully provide curated fallback styling recommendations.

---

## 3. System Architecture & Feasible Tech Stack

| Layer | Selected Technology | Grounded Rationale |
| :--- | :--- | :--- |
| **Frontend Framework** | React 19 (Vite) | High performance, component reusability, instant Hot Module Reloading. |
| **Styling** | Modern CSS3 (Vanilla + Glassmorphism) | High aesthetic fidelity without heavy dependencies or build-chain fragility. |
| **Icons** | Lucide React | Lightweight, scalable SVG icons. |
| **AI Integration** | Google Gemini Flash API (or client proxy) | Generous free tier, fast inference, high-quality fashion reasoning. |
| **Fallback Engine** | Curated JSON Recommendation Catalog | Guarantees instant offline responses during live college viva demonstrations. |
| **Storage** | Browser `localStorage` | Zero-configuration persistence for chat history and user style profiles. |
| **Deployment** | Vercel / Netlify / GitHub Pages | Free, instant SSL, zero server management. |

---

## 4. Functional Requirements

### 4.1 Module 1: Conversational Styling Interface
- **FR-1.1:** The system shall display a clean conversation stream differentiating between user queries and VogueBot recommendations.
- **FR-1.2:** The bot shall generate contextual, friendly styling advice specifying garments, accessories, footwear, and color palettes.
- **FR-1.3:** The system must show realistic typing indicators / loading states while formulating recommendations.
- **FR-1.4:** The input box must support auto-clearing, submit on Enter, and input validation to prevent empty submissions.

### 4.2 Module 2: Visual Outfit Recommendation Cards
- **FR-2.1:** When an outfit is suggested, the bot shall attach an Outfit Card featuring:
  - High-resolution outfit preview image.
  - Outfit title (e.g., "Midnight Glamour", "Urban Casual").
  - Aesthetic and occasion tags (e.g., `#Party`, `#Modern`, `#Monochrome`).
  - Itemized garment breakdown or styling note.
- **FR-2.2:** Outfit cards must be responsive and adapt seamlessly between mobile and widescreen viewports.

### 4.3 Module 3: Style Presets & Occasion Navigation
- **FR-3.1:** The sidebar shall provide quick-action buttons for common occasions:
  - *Casual / College Wear*
  - *Corporate / Modern Office*
  - *Party / Evening Glamour*
  - *Ethnic / Traditional / Festive*
  - *Retro & Vintage Aesthetics*
- **FR-3.2:** Clicking a preset shall populate or trigger a prompt directly in the chat stream.

### 4.4 Module 4: Session & History Management
- **FR-4.1:** Users shall be able to start a "New Styling Session", resetting the active conversation view.
- **FR-4.2:** Past sessions shall be saved in the sidebar and persist across page refreshes via `localStorage`.
- **FR-4.3:** Users shall have the ability to clear or delete past styling conversations.

### 4.5 Module 5: User Preferences & Wardrobe Filters
- **FR-5.1:** Allow users to set baseline style preferences (e.g., Minimalist, Streetwear, Bohemian, Gender preference: Men/Women/Androgynous).
- **FR-5.2:** Recommendations must adjust context based on the active user preference tags.

---

## 5. Non-Functional Requirements

### 5.1 Performance
- **Initial Page Load:** Under 1.5 seconds on standard broadband connections.
- **Response Latency:** Offline/fallback bot responses under 600ms; AI API responses under 2.5 seconds.
- **Smooth Animations:** 60fps micro-transitions on hover states, message bubble entries, and modal transitions.

### 5.2 Usability & Aesthetics
- **Theme:** Sleek, modern dark-mode aesthetic with glassmorphic translucent panels (`backdrop-filter: blur`), subtle gradients, and high-contrast typography.
- **Intuitiveness:** Zero-learning curve chat interface modeled on popular conversational agents.

### 5.3 Reliability & Fault Tolerance
- **Graceful Degradation:** If the external AI API fails or hits rate quotas, the system automatically falls back to internal fashion rule sets without breaking the UI.
- **Input Sanitization:** Sanitize user input to prevent XSS (Cross-Site Scripting).

### 5.4 Maintainability
- Clean component separation (`Sidebar`, `ChatArea`, `OutfitCard`, `PresetList`, `PreferencesModal`).
- Modular state management using React hooks (`useState`, `useEffect`, `useCallback`).

---

## 6. Data Model & Schema (Client-Side JSON)

### 6.1 Message Object
```json
{
  "id": "msg_1711200000000",
  "sender": "bot" | "user",
  "text": "String description of outfit or style advice",
  "timestamp": "14:32",
  "outfit": {
    "id": "outfit_01",
    "name": "Midnight Glamour",
    "description": "Tailored velvet blazer paired with satin slip dress and ankle boots.",
    "tags": ["Evening", "Party", "Chic"],
    "image": "https://images.unsplash.com/..."
  }
}
```

### 6.2 Session Object
```json
{
  "id": "session_01",
  "title": "Retro 70s Party",
  "createdAt": "2026-09-26T15:30:00Z",
  "messages": []
}
```

### 6.3 Style Profile Object
```json
{
  "gender": "Universal",
  "preferredAesthetics": ["Minimalist", "Streetwear"],
  "avoidColors": ["Neon"],
  "budgetTier": "Mid-range"
}
```

---

## 7. Verification & Acceptance Criteria
1. **Core Chat:** User can type prompt -> Receive relevant styling guidance -> View visual outfit card.
2. **Sidebar Interactivity:** Clicking past sessions or presets updates the chat stream without page reloads.
3. **Data Persistence:** Reloading browser preserves previous messages.
4. **Responsive Layout:** Layout cleanly collapses into a mobile drawer / bottom sheet on viewports `< 768px`.
5. **Viva Readiness:** Code contains clean inline documentation, no broken external image links, and runs with `npm run dev` out of the box.
