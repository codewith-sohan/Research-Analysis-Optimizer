# Research Application Optimizer (RAO)

## Project Documentation

**Version:** 2.0 (AI-Powered)

---

## 1. Executive Summary

Research Application Optimizer (RAO) is an AI-driven web platform designed to streamline and formalize the research application process between students and professors. Unlike traditional job boards or informal cold-email approaches, RAO leverages Generative AI to analyze research papers, lab websites, and academic contexts to help students generate highly personalized and professional outreach emails and cover letters.

The platform provides two dedicated interfaces:

* A **Student Optimizer Toolkit** for discovering research opportunities, analyzing papers, and crafting AI-assisted applications.
* A **Professor Management Dashboard** for posting research opportunities, reviewing applicants, and managing recruitment efficiently.

RAO aims to reduce unstructured communication, improve application quality, and increase the likelihood of meaningful research collaborations.

---

## 2. Key Objectives

* Bridge the communication gap between students and professors
* Improve the quality of research outreach using AI personalization
* Reduce manual screening effort for professors
* Provide structured application tracking and transparency
* Enable scalable academic recruitment workflows

---

## 3. Technical Stack

The application is implemented as a monolithic Flask-based web system with modular components.

### Backend

* **Language:** Python
* **Framework:** Flask
* **ORM:** Flask-SQLAlchemy
* **Authentication:** Flask-Login (session-based)

### Database

* **Primary Database:** SQLite (development)
* **Schema:** Relational

### AI and External Services

* **Generative AI:** Google Gemini API (gemini-1.5-flash / gemini-pro)
* **Research Paper Sources:**

  * ArXiv API (trending and recent papers)
  * Semantic Scholar (metadata extraction)
* **Context Retrieval:** DuckDuckGo Search (lab and professor context)

### Frontend

* HTML5 (Jinja2 templates)
* CSS3 (Flexbox and Grid-based layouts)
* JavaScript (Fetch API for asynchronous AI operations)

---

## 4. Installation and Configuration

### 4.1 Prerequisites

* Python 3.8 or higher
* Google Gemini API Key
* pip package manager
* (Optional) Python virtual environment

### 4.2 Setup Instructions

#### Step 1: Clone the Repository

Extract or clone the project files into a local directory (e.g., `RAO_FINAL`).

#### Step 2: Install Dependencies

Install all required Python packages:

```
pip install -r requirements.txt
```

#### Step 3: Environment Configuration

Create a `.env` file in the project root and configure the following variables:

```
GEMINI_API_KEY=your_google_gemini_api_key_here
DATABASE_URL=sqlite:///devsync.db
```

#### Step 4: Database Initialization

Initialize and seed the database with demo data:

```
python seed.py
```

This script creates the database file and adds a default professor account:

* Email: [prof@mit.edu](mailto:prof@mit.edu)
* Password: 123

#### Step 5: Run the Application

Start the Flask development server:

```
python app.py
```

The application will be available at `http://127.0.0.1:5000/`.

---

## 5. System Architecture

### 5.1 Project Structure

```
RAO/
│── app.py            # Main application controller and routes
│── models.py         # Database schema definitions
│── seed.py           # Database seeding script
│── templates/        # Jinja2 HTML templates
│   ├── layout.html   # Base layout
│   ├── student.html  # Student optimizer interface
│   └── professor.html# Professor dashboard
│── static/
│   ├── uploads/      # Resume uploads
│   └── css, js/      # Static assets
```

### 5.2 Database Schema

#### User Table

Stores authentication and profile information.

* Fields: `id`, `email`, `password`, `role`, `full_name`
* Student-specific: `qualification`, `college`, `resume_file`

#### Internship Table

Represents research opportunities or paper-based collaborations.

* Fields: `id`, `title`, `domain`, `description`, `type`, `pdf_link`, `vacancies`
* Foreign Key: `user_id` (Professor)

#### Application Table

Tracks student applications.

* Fields: `id`, `status`, `cover_letter`
* Foreign Keys: `student_id`, `internship_id`

---

## 6. Core Logic and AI Pipeline

The `optimize()` function in `app.py` forms the core intelligence of RAO.

### Pipeline Steps

1. **Input Acquisition**: Paper URL or text description provided by the student
2. **Metadata Extraction**:

   * Fetches title, abstract, authors using ArXiv or Semantic Scholar
   * Attempts PI/professor identification
3. **Context Scraping**:

   * Searches professor lab websites via DuckDuckGo
   * Extracts textual context to infer research focus
4. **AI Generation (Gemini)**:

   * Constructs a structured prompt combining:

     * Paper abstract
     * Lab context
     * Student profile
   * Outputs a structured JSON containing:

     * Paper summary
     * Required skills
     * Research fit analysis
     * Personalized cold email draft

---

## 7. User Workflows

### 7.1 Student Workflow

* Access Optimizer Dashboard (`/student`)
* Input paper link or opportunity description
* Trigger AI optimization
* Review generated summary, skills, and email
* Edit and submit application
* Track status via `/my_applications`

### 7.2 Professor Workflow

* Access Dashboard (`/professor`)
* Post or auto-generate opportunities from ArXiv
* Review applicants via `/view_applicants`
* Download resumes and review cover letters
* Update application status (Selected / Pending)
* Manage cold applications via dedicated inbox

---

## 8. Feature Overview

| Feature               | Description                                | Technology           |
| --------------------- | ------------------------------------------ | -------------------- |
| Smart Drafting        | Context-aware personalized research emails | Gemini AI            |
| Lab Contextualization | Professor research focus extraction        | DuckDuckGo, Scraping |
| Resume Management     | Secure PDF upload and retrieval            | Flask Uploads        |
| Auto Research Feed    | AI-based trending paper ingestion          | ArXiv API            |
| Application Tracking  | Status-based monitoring                    | Flask + SQLAlchemy   |

---

## 9. Security Considerations

* Role-based access control
* Secure handling of API keys via environment variables
* Controlled file upload handling

---

## 10. Limitations

* SQLite database limits scalability
* Heuristic-based professor identification
* No real-time notifications

---

## 11. Future Enhancements

* ML-based research matching engine
* Google Scholar integration
* Notification and messaging system
* Multi-university deployment support
* Advanced analytics for faculty

---

## 12. Troubleshooting

* Verify `GEMINI_API_KEY` configuration
* Avoid external access to SQLite DB during runtime
* Ensure `static/uploads` directory permissions
* Review Flask logs for runtime errors

