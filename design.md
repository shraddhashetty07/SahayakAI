# SahayakAI - Design Document (Hackathon MVP)

## 1. System Overview

SahayakAI is a web-based application that helps Indian citizens discover government welfare schemes they're eligible for. Users input their demographic details (age, income, gender, category, state), and the system matches them with relevant schemes using simple rule-based logic.

### Key Features
- User-friendly form for demographic input
- Rule-based eligibility matching
- Clear explanation of eligibility decisions
- Document checklist for eligible schemes
- English and Hindi language support

### Target Outcome
A working prototype that demonstrates the core concept of automated scheme discovery and eligibility assessment.

## 2. High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         USER                                 │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                    FRONTEND (Web UI)                         │
│  - Input Form (age, income, gender, category, state)        │
│  - Results Display (eligible schemes)                        │
│  - Language Toggle (EN/HI)                                   │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                  BACKEND (Flask/Express)                     │
│  ┌──────────────────────────────────────────────────────┐   │
│  │         Eligibility Engine (Rule-Based)              │   │
│  │  - Match user profile with scheme criteria           │   │
│  │  - Generate eligibility explanations                 │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │         Translation Module (Optional)                │   │
│  │  - Translate UI and scheme info to Hindi             │   │
│  └──────────────────────────────────────────────────────┘   │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│              DATA STORAGE (JSON File)                        │
│  - schemes.json (5-10 government schemes)                    │
│  - translations.json (English/Hindi mappings)                │
└─────────────────────────────────────────────────────────────┘
```

## 3. Component Description

### 3.1 Frontend (Web UI)

**Technology**: HTML, CSS, JavaScript (or React for familiarity)

**Components**:
- **Input Form**: Collects user demographics
  - Age (number input)
  - Annual Income (number input)
  - Gender (dropdown: Male/Female/Other)
  - Category (dropdown: General/SC/ST/OBC)
  - State (dropdown: list of Indian states)
  - Submit button

- **Results Section**: Displays matched schemes
  - Scheme cards showing:
    - Scheme name
    - Benefits description
    - Eligibility status (✓ Eligible / ✗ Not Eligible)
    - Reason for eligibility/ineligibility
    - Required documents list

- **Language Toggle**: Switch between English and Hindi
  - Simple button to change UI language
  - Updates all text labels and scheme information

**Styling**: Bootstrap or Tailwind CSS for quick, responsive design

### 3.2 Backend (Eligibility Engine)

**Technology**: Node.js + Express

**Core Functions**:

1. **`check_eligibility(user_profile, scheme)`**
   - Input: User demographics + scheme eligibility criteria
   - Logic: Compare user attributes with scheme requirements
   - Output: Boolean (eligible/not eligible) + reason string

2. **`get_matching_schemes(user_profile)`**
   - Input: User demographics
   - Process: Loop through all schemes, check eligibility for each
   - Output: List of schemes with eligibility status and reasons

3. **`generate_document_list(eligible_schemes)`**
   - Input: List of eligible schemes
   - Process: Collect unique documents from all eligible schemes
   - Output: Consolidated document checklist

4. **`translate_content(text, target_language)`** (Optional)
   - Input: English text + target language (Hindi)
   - Process: Use translation dictionary or API
   - Output: Translated text

**API Endpoints**:
- `POST /api/check-eligibility` - Accepts user profile, returns eligible schemes
- `GET /api/schemes` - Returns all available schemes
- `GET /api/translate/:lang` - Returns translations for UI elements

### 3.3 Data Storage (JSON Files)

**schemes.json** - Contains government scheme information

```json
[
  {
    "id": "pm-kisan",
    "name": "PM-KISAN",
    "name_hi": "पीएम-किसान",
    "description": "Financial support to farmers",
    "description_hi": "किसानों को वित्तीय सहायता",
    "benefits": "₹6000 per year in 3 installments",
    "benefits_hi": "₹6000 प्रति वर्ष 3 किस्तों में",
    "eligibility_criteria": {
      "min_age": 18,
      "max_age": null,
      "income_limit": 200000,
      "gender": ["Male", "Female", "Other"],
      "category": ["General", "SC", "ST", "OBC"],
      "states": ["all"],
      "occupation": "farmer"
    },
    "documents": [
      "Aadhaar Card",
      "Land Ownership Documents",
      "Bank Account Details"
    ],
    "documents_hi": [
      "आधार कार्ड",
      "भूमि स्वामित्व दस्तावेज",
      "बैंक खाता विवरण"
    ]
  }
]
```

**translations.json** - UI text translations

```json
{
  "en": {
    "title": "SahayakAI - Welfare Scheme Assistant",
    "age_label": "Age",
    "income_label": "Annual Income (₹)",
    "submit_button": "Find Schemes"
  },
  "hi": {
    "title": "सहायकAI - कल्याण योजना सहायक",
    "age_label": "आयु",
    "income_label": "वार्षिक आय (₹)",
    "submit_button": "योजनाएं खोजें"
  }
}
```

## 4. Data Flow (Step-by-Step)

### User Journey Flow

```
1. User opens web application
   ↓
2. User selects language (English/Hindi)
   ↓
3. User fills demographic form:
   - Age: 65
   - Income: ₹150,000
   - Gender: Male
   - Category: General
   - State: Maharashtra
   ↓
4. User clicks "Find Schemes" button
   ↓
5. Frontend sends POST request to backend with user data
   ↓
6. Backend receives user profile
   ↓
7. Eligibility Engine loops through schemes.json:
   
   For each scheme:
   a. Check age criteria (65 >= min_age AND 65 <= max_age)
   b. Check income criteria (150000 <= income_limit)
   c. Check gender match
   d. Check category match
   e. Check state match
   f. Generate reason string
   
   ↓
8. Backend compiles results:
   - Eligible schemes with reasons
   - Not eligible schemes with reasons
   - Document checklist for eligible schemes
   ↓
9. Backend sends JSON response to frontend
   ↓
10. Frontend displays results:
    - Green cards for eligible schemes
    - Red cards for not eligible schemes
    - Document checklist section
    ↓
11. User views results and document requirements
```

### Example Request/Response

**Request** (POST /api/check-eligibility):
```json
{
  "age": 65,
  "income": 150000,
  "gender": "Male",
  "category": "General",
  "state": "Maharashtra"
}
```

**Response**:
```json
{
  "eligible_schemes": [
    {
      "scheme_id": "pm-kisan",
      "scheme_name": "PM-KISAN",
      "benefits": "₹6000 per year",
      "reason": "You are eligible because your age is 65 and income is ₹150,000 (below ₹200,000 limit)",
      "documents": ["Aadhaar Card", "Land Ownership Documents", "Bank Account Details"]
    }
  ],
  "not_eligible_schemes": [
    {
      "scheme_id": "beti-bachao",
      "scheme_name": "Beti Bachao Beti Padhao",
      "reason": "Not eligible: This scheme is for female children only"
    }
  ],
  "document_checklist": ["Aadhaar Card", "Land Ownership Documents", "Bank Account Details"]
}
```

## 5. AI Logic Design (Rule-Based Approach)

### Eligibility Matching Algorithm

**Approach**: Simple conditional logic (if-else statements)

**Pseudocode**:

```python
def check_eligibility(user, scheme):
    reasons = []
    eligible = True
    
    # Check age criteria
    if scheme.min_age and user.age < scheme.min_age:
        eligible = False
        reasons.append(f"Age must be at least {scheme.min_age}")
    
    if scheme.max_age and user.age > scheme.max_age:
        eligible = False
        reasons.append(f"Age must be below {scheme.max_age}")
    
    # Check income criteria
    if scheme.income_limit and user.income > scheme.income_limit:
        eligible = False
        reasons.append(f"Income must be below ₹{scheme.income_limit}")
    
    # Check gender criteria
    if scheme.gender and user.gender not in scheme.gender:
        eligible = False
        reasons.append(f"This scheme is for {scheme.gender} only")
    
    # Check category criteria
    if scheme.category and user.category not in scheme.category:
        eligible = False
        reasons.append(f"This scheme is for {scheme.category} categories")
    
    # Check state criteria
    if scheme.states != ["all"] and user.state not in scheme.states:
        eligible = False
        reasons.append(f"This scheme is not available in {user.state}")
    
    # Generate explanation
    if eligible:
        explanation = f"You are eligible! Your profile matches all criteria."
    else:
        explanation = "Not eligible: " + ", ".join(reasons)
    
    return {
        "eligible": eligible,
        "reason": explanation
    }
```

### Explanation Generation

**Simple Template-Based Approach**:

For eligible schemes:
```
"✓ You are eligible for {scheme_name} because:
- Your age ({user_age}) meets the requirement ({min_age}-{max_age})
- Your income (₹{user_income}) is below the limit (₹{income_limit})
- Your category ({user_category}) is covered"
```

For ineligible schemes:
```
"✗ You are not eligible for {scheme_name} because:
- {reason_1}
- {reason_2}"
```

### Optional: LLM Enhancement

If time permits, use a free LLM API (OpenAI/Gemini) to generate more natural explanations:

```python
def generate_llm_explanation(user, scheme, eligible):
    prompt = f"""
    User Profile: Age {user.age}, Income ₹{user.income}, {user.gender}, {user.category}
    Scheme: {scheme.name}
    Eligibility: {eligible}
    
    Explain in simple language why the user is {'eligible' if eligible else 'not eligible'}.
    """
    
    response = llm_api.generate(prompt)
    return response
```

## 6. Multilingual Support Design

### Approach: Static Translation (English ↔ Hindi)

**Implementation Strategy**:

1. **Store bilingual content in JSON**
   - Each scheme has `name` and `name_hi` fields
   - Each field has English and Hindi versions

2. **Frontend language state**
   - JavaScript variable: `currentLanguage = 'en'` or `'hi'`
   - Toggle button switches between languages

3. **Dynamic content rendering**
   ```javascript
   function displayScheme(scheme, language) {
       const name = language === 'hi' ? scheme.name_hi : scheme.name;
       const description = language === 'hi' ? scheme.description_hi : scheme.description;
       // Render with appropriate language
   }
   ```

4. **UI element translation**
   - Load translations from `translations.json`
   - Replace text content on language switch
   ```javascript
   document.getElementById('title').textContent = translations[currentLanguage].title;
   ```

### Translation Coverage

**What to translate**:
- UI labels (Age, Income, Submit button, etc.)
- Scheme names and descriptions
- Eligibility reasons (use templates)
- Document names

**What NOT to translate** (for MVP):
- User input values
- Error messages (keep in English for simplicity)

### Fallback Strategy

If Hindi translation is missing:
- Display English version
- Add "(English)" indicator

## 7. Limitations of MVP

### Technical Limitations
- Only 5-10 hardcoded schemes (not comprehensive)
- Rule-based logic only (no ML or adaptive learning)
- No user authentication or profile saving
- No real-time scheme updates
- Limited to 2 languages (English and Hindi)
- No mobile app (web only)

### Data Limitations
- Scheme data is static (manually curated)
- May not reflect latest scheme updates
- Limited state-specific schemes
- Simplified eligibility criteria (real schemes are more complex)

### Functional Limitations
- No direct application submission
- No document upload or verification
- No scheme comparison feature
- No personalized recommendations
- No search or filter functionality

### Scalability Limitations
- JSON file storage (not suitable for large datasets)
- No caching mechanism
- Single-server deployment
- No load balancing

## 8. Future Improvements

### Short-term (Post-Hackathon)
- Add 20-30 more schemes covering all major categories
- Implement search and filter functionality
- Add more Indian languages (Tamil, Telugu, Bengali)
- Create mobile-responsive design
- Add scheme comparison feature

### Medium-term
- Integrate with government APIs for real-time scheme data
- Add user authentication and profile saving
- Implement chatbot interface for natural language queries
- Add document upload and verification
- Create mobile app (React Native/Flutter)

### Long-term
- Use ML for personalized scheme recommendations
- Integrate with Aadhaar for auto-profile population
- Add application tracking and status updates
- Partner with government agencies for official integration
- Implement voice-based assistance for accessibility

---

**Document Version**: 1.0 (Hackathon MVP)  
**Last Updated**: February 2026  
**Team Size**: 2 members  
**Project Status**: Idea Submission Phase


