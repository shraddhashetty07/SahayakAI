# SahayakAI - Hackathon MVP Requirements

## Problem Statement

Indian citizens struggle to discover government welfare schemes they're eligible for due to:
- Lack of awareness about available schemes
- Complex eligibility criteria (age, income, gender, category, state)
- Difficulty understanding required documents
- Time-consuming manual search

## Objective

Build a simple AI assistant that:
- Finds eligible government schemes based on user input
- Explains why they're eligible or not
- Lists required documents for application
- Works in English and Hindi

## Target Users

- Indian citizens looking for welfare schemes
- Focus on basic digital literacy users
- English and Hindi speakers

## Functional Requirements (MVP)

### Must Have
1. **User Input Form**
   - Simple form to collect: age, income, gender, category (SC/ST/OBC/General), state
   - No login required

2. **Scheme Matching**
   - Show 5-10 popular government schemes (hardcoded dataset)
   - Display scheme name, benefits, and basic details
   - Highlight eligible vs not eligible

3. **Eligibility Check**
   - Simple rule-based matching (if-else logic)
   - Show "Eligible" or "Not Eligible" with reason
   - Example: "You are eligible because your age is 65+ and income is below ₹2 lakh"

4. **Document List**
   - Show required documents for eligible schemes
   - Simple bullet list (Aadhaar, income certificate, etc.)

5. **Language Toggle**
   - Switch between English and Hindi
   - Translate UI labels and scheme names

### Nice to Have (if time permits)
- Search schemes by keyword
- Download results as PDF
- Chat interface instead of form

## Non-Functional Requirements

### Keep It Simple
- Works on laptop/desktop browser (mobile-responsive is bonus)
- Fast enough for demo (no specific performance targets)
- Clean, easy-to-use interface
- Basic error handling

## AI Component Description

### Eligibility Logic (Rule-Based)
- Simple if-else conditions to check eligibility
- Example: `if age >= 60 and income < 200000: eligible = True`
- No ML models needed - just hardcoded rules

### Optional: LLM Integration
- Use free API (OpenAI, Gemini, or Groq) to explain eligibility in natural language
- Example prompt: "Explain why this user is eligible for PM-KISAN scheme"
- Fallback to template-based text if API fails

### Language Support
- Use Google Translate API (free tier) or hardcoded translations
- Translate only key UI elements and scheme names

## Tech Stack (Beginner-Friendly)

### Frontend
- **HTML/CSS/JavaScript** (vanilla or React if comfortable)
- **Bootstrap** or **Tailwind CSS** for quick styling
- Simple form-based UI

### Backend (Choose One)
- **Option 1**: Python + Flask (easiest for beginners)
- **Option 2**: Node.js + Express
- **Option 3**: No backend - pure frontend with hardcoded data in JSON

### Data Storage
- **JSON file** with 5-10 schemes (no database needed)
- Store scheme details: name, eligibility rules, documents, benefits

### AI/LLM (Optional)
- **Free APIs**: OpenAI (trial credits), Google Gemini, or Groq
- Use for generating explanations only
- Keep it simple - not required for MVP

### Deployment
- **Frontend**: Vercel, Netlify, or GitHub Pages (free)
- **Backend**: Render, Railway, or Replit (free tier)
- No Docker/Kubernetes needed

## Sample Scheme Data Structure

```json
{
  "scheme_name": "PM-KISAN",
  "description": "Financial support to farmers",
  "benefits": "₹6000 per year",
  "eligibility": {
    "occupation": "farmer",
    "land_ownership": true,
    "income_limit": 200000
  },
  "documents": ["Aadhaar", "Land records", "Bank account"]
}
```

## Hackathon Timeline (2-3 days)

### Day 1
- Set up project structure
- Create JSON file with 5-10 schemes
- Build basic HTML form for user input
- Implement eligibility logic (if-else rules)

### Day 2
- Display eligible schemes with reasons
- Show document checklist
- Add English/Hindi toggle
- Style the UI

### Day 3
- Test and fix bugs
- Optional: Add LLM for explanations
- Prepare demo and presentation
- Deploy to free hosting

## Success Criteria

- User can input their details
- System shows 3-5 eligible schemes
- Clear explanation of why they're eligible
- Document list is displayed
- Works in English (Hindi is bonus)
- Deployed and accessible via link

## Future Scope (Post-Hackathon)

- Add more schemes and states
- Better UI/UX with animations
- Chat interface with AI
- Mobile app version
- Real government API integration

---

**Document Version**: 1.0 (Hackathon MVP)  
**Last Updated**: February 2026  
**Team Size**: 2 members  
**Project Phase**: Idea Submission Round


