AROGYINI is a **women-focused web app for India** that puts four kinds of help in one place: **health, legal rights, careers, and emergency safety**. It also has an AI assistant that answers questions in these areas. 

### The four pillars

**1. Health (`Health.jsx`)**

- **Period/cycle tracker:** you log flow, symptoms, mood and notes. The server calculates your cycle day, phase (menstrual, follicular, ovulation, luteal), next period date and ovulation date.
- **PCOS / anemia / thyroid risk screener:** you enter age, BMI, cycle regularity, fatigue, stress, hemoglobin and family history, and it returns a risk score with suggested next steps.
- Static content on phase-based nutrition and mental health, plus a list of gynecologists with call buttons.

**2. Legal (`Legal.jsx`)**

- Explainers for Indian laws: the POSH Act 2013, the Domestic Violence Act 2005, the Maternity Benefit Act, and the cyber/IT Act.
- A **complaint drafter**: you fill in names, date, place and what happened, and it generates a formatted POSH complaint, Zero FIR application, or Domestic Violence Act application that you can copy.
- Free legal aid numbers (NALSA 15100, NCW).

**3. Career (`Career.jsx`)**

- A job board with women-centric roles and returnships (programs for women returning after a career break).
- Scholarships and grants such as AICTE Pragati, Google, and Stand-Up India.
- Resume tips for career gaps and mentorship circles.

**4. Safety (`Safety.jsx`, `SOSButton.jsx`)**

- A **floating SOS button** on every page. It gives a 3-second countdown (to prevent accidental taps), gets your GPS location, and logs an emergency event that notifies your guardians and police (112).
- A **siren** generated in the browser with the Web Audio API.
- A **fake incoming call** screen to give you an excuse to leave an uncomfortable situation.
- Management of emergency contacts ("guardians").
- A "safe zone radar" listing police booths, hospitals and crisis centres, with a "safety score" for the area.
  
  
```
  Browser (React + Tailwind)
   │  fetch /api/...
   ▼
Node/Express server (server.js, port 3000)
   ├── routes → controllers → models → in-memory "db" (server/config/db.js)
   ├── ragService ──► optional Python RAG service (RAG_SERVICE_URL, port 8000)
   │               └► else: built-in keyword retrieval + Gemini (if GEMINI_API_KEY) or a template answer
   └── mlService  ──► optional Python ML service (ML_SERVICE_URL, port 8001)
                   └► else: built-in rule-based scoring
```