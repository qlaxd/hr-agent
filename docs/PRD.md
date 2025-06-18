# PRD: Resourcing AI Agent

## 1. Product overview
### 1.1 Document title and version
- PRD: Resourcing AI Agent
- Version: 1.0

### 1.2 Product summary
This document outlines the requirements for an internal AI-powered application designed to support the Talent Consultant team at KIBT Solutions. The tool will integrate with the existing candidate database stored in Google Workspace to streamline and enhance the recruitment process.

The core functionality includes matching candidates to specific job profiles by providing a relevance score and a concise evaluation for each. Additionally, it will feature a "helpdesk" function, allowing recruiters to ask natural language questions about the candidate pool. The initial version will focus on core functionality with a simple user interface, ensuring data security and compliance with KIBT's data policies are top priorities.

## 2. Goals
### 2.1 Business goals
- Increase the efficiency of the recruitment team.
- Reduce the time-to-fill for open positions by accelerating the candidate sourcing phase.
- Improve the quality and accuracy of candidate-to-job matching.
- Maximize the value and utilization of the existing internal candidate database.

### 2.2 User goals
- To quickly find the most relevant candidates for a given job profile without manual searching.
- To get a quick, summarized understanding of a candidate's suitability for a role.
- To easily query the database for specific information, such as the number of candidates with a particular skill.

### 2.3 Non-goals
- This tool is not intended to replace the human element of recruiting; it is an assistive tool.
- The application will not handle direct communication with candidates.
- It will not manage the full recruitment lifecycle (e.g., interview scheduling, offer management).
- A complex user interface is not a goal for the initial release.
- The system will not store any candidate data outside of the approved, secure hosting environment.

## 3. User personas
### 3.1 Key user types
- Talent Consultant (Recruiter)
- System Administrator

### 3.2 Basic persona details
- **Talent Consultant**: A member of the KIBT Solutions recruitment team. They are responsible for sourcing, screening, and presenting candidates for a variety of technical and non-technical roles. They need tools that are fast, efficient, and accurate to meet hiring demands.
- **System Administrator**: A technical staff member responsible for the deployment, maintenance, security, and performance of the application.

### 3.3 Role-based access
- **Talent Consultant**: Has full access to the application's features, including searching for candidates, using the helpdesk Q&A, and viewing all results. Access to the underlying data is read-only.
- **System Administrator**: Manages system configuration, monitors performance, ensures security compliance, and handles the integration with Google Workspace. They do not have access to the UI for recruitment tasks but oversee the system's health.

## 4. Functional requirements
- **Candidate Search & Matching** (Priority: High)
  - Users must be able to input a job profile description into the system.
  - The system will process the input and search the connected Google Workspace database.
  - The output will be a ranked list of relevant candidates, each with a relevance score and a short, AI-generated summary.
- **Helpdesk Q&A** (Priority: High)
  - A natural language query interface for asking questions about the candidate database.
  - The system must be able to understand and answer questions like, "Are there any candidates with Kubernetes knowledge?" or "How many candidates have we interviewed for developer roles?"
- **Simple User Interface** (Priority: Medium)
  - A clean, web-based UI with clear input fields for the job profile and helpdesk questions.
  - Results should be displayed in a clean, easy-to-read format.
- **Security & Data Privacy** (Priority: High)
  - The system must require user authentication to ensure only authorized personnel have access.
  - It must strictly adhere to KIBT's data policy, with no personal data leaving the secure environment.
  - Any external data processing must be done in a controlled and anonymized manner, with prior approval.
- **System Hosting** (Priority: High)
  - The application must be deployable on Google Cloud or a local on-premise environment to maintain data security.

## 5. User experience
### 5.1. Entry points & first-time user flow
- Users access the tool via a dedicated internal URL.
- First-time access requires a one-time login, preferably via SSO with their company Google account.
- Upon login, the user is presented with a simple dashboard with the two main features clearly visible.

### 5.2. Core experience
- **Step 1: Accessing the tool**: The user logs in and sees a clean dashboard with two primary functions: "Search Candidates" and "Ask Helpdesk."
  - The interface is designed to be intuitive and require minimal training.
- **Step 2: Searching for candidates**: The user pastes a full job description into the "Search Candidates" input field and clicks a "Search" button.
  - The system provides immediate visual feedback that the search is in progress.
- **Step 3: Reviewing results**: A ranked list of candidates is displayed. Each item in the list shows the candidate's name, a relevance score, and a concise summary of their qualifications.
  - The summary highlights the specific skills and experiences from their documents that align with the job profile.
- **Step 4: Using the Helpdesk**: The user types a question into the "Ask Helpdesk" field and hits enter.
  - The system provides a direct, text-based answer to the question.

### 5.3. Advanced features & edge cases
- For ambiguous queries, the system should either return the best possible result with a disclaimer or ask for clarification.
- If a search yields no results, a clear "No results found" message will be displayed.
- The system must gracefully handle various document formats, including PDF, DOCX, and DOC.
- User-friendly error messages will be displayed in case of connectivity issues with the database.

### 5.4. UI/UX highlights
- A minimalist, clean, and function-oriented design.
- Emphasis on fast response times for all searches and queries.
- A clear, uncluttered presentation of results to facilitate quick decision-making.
- A single-page application experience to avoid unnecessary page loads and navigation.

## 6. Narrative
Anna is a Talent Consultant at KIBT Solutions who needs to quickly find the best candidates for a new Senior Java Developer role. Previously, she had to manually search through dozens of folders and CVs in Google Workspace, which was slow and tedious. She now opens this tool, pastes the job description into the search box, and within seconds gets a ranked list of the top 5 candidates from their database, complete with a relevance score and a summary for each. She then uses the helpdesk feature to ask, "How many of these have experience with cloud platforms?" and gets an instant answer. This tool allows her to identify the most promising candidates in minutes instead of hours, significantly speeding up her workflow and improving the quality of her submissions.

## 7. Success metrics
### 7.1. User-centric metrics
- High adoption rate and frequent use by the Talent Consultant team.
- Positive qualitative feedback gathered through user surveys and interviews.
- Measurable reduction in time spent per sourcing task.
- High number of daily queries per user.

### 7.2. Business metrics
- Reduction in the average time-to-fill for open positions.
- Increase in the percentage of submitted candidates who are invited for an interview.
- Increased placement rate from the existing internal candidate database.

### 7.3. Technical metrics
- Average query and search response time remains below a set threshold.
- High system uptime and availability (e.g., 99.9%).
- Zero data leakage or security incidents.

## 8. Technical considerations
### 8.1. Integration points
- Google Workspace API for secure access to documents in Google Drive.
- Google Identity Platform (OAuth 2.0) for single sign-on (SSO) authentication.

### 8.2. Data storage & privacy
- All original candidate documents will remain within the secure Google Workspace environment.
- The application will process data in-memory or in a secure, temporary storage location within the approved hosting environment.
- A strict data anonymization process for all personally identifiable information (PII) must be implemented and approved before using any external services (e.g., LLM APIs).
- The system must be fully compliant with GDPR and KIBT's internal data security policies.

### 8.3. Scalability & performance
- The architecture must be designed to handle the current and future size of the candidate database without significant performance degradation.
- The system must support concurrent usage by all members of the Talent Consultant team.
- An efficient document indexing and retrieval strategy (e.g., a vector database) should be implemented to ensure fast search results.

### 8.4. Potential challenges
- Accurately parsing and extracting structured information from a wide variety of unstructured CV formats.
- Ensuring the AI-generated summaries are factually accurate, relevant, and free of bias.
- Maintaining strict data security and privacy while potentially leveraging external AI models.
- Selecting the optimal hosting solution (Google Cloud vs. local) by balancing cost, security, and maintenance overhead.

## 9. Milestones & sequencing
### 9.1. Project estimate
- Medium: 4-6 weeks for the initial version (MVP).

### 9.2. Team size & composition
- Small Team: 2-4 people
  - 1 Product Manager, 1-2 AI/Backend Engineers, 1 Frontend Engineer.

### 9.3. Suggested phases
- **Phase 1**: Backend & Core Logic (2-3 weeks)
  - Key deliverables: Secure connection to Google Workspace, document parsing pipeline, core search and matching algorithm, and initial helpdesk query logic.
- **Phase 2**: UI & Integration (2-3 weeks)
  - Key deliverables: A simple web interface, user authentication, and the end-to-end integration of the frontend and backend.
- **Phase 3**: Testing & Rollout (1 week)
  - Key deliverables: User Acceptance Testing (UAT) with the Talent Consultant team, bug fixing, and the official internal launch.

## 10. User stories
### 10.1. Secure user login
- **ID**: US-001
- **Description**: As a Talent Consultant, I want to securely log in to the application so that only authorized personnel can access candidate data.
- **Acceptance criteria**:
  - The application provides a login mechanism.
  - Users can authenticate using their company Google Workspace credentials (SSO).
  - Users without valid credentials are denied access.
  - All access attempts (successful and failed) are logged.

### 10.2. Search candidates by job profile
- **ID**: US-002
- **Description**: As a Talent Consultant, I want to provide a job profile and receive a list of relevant candidates so that I can quickly identify potential hires.
- **Acceptance criteria**:
  - The UI has a text area where a job profile can be entered.
  - A "Search" button initiates the candidate search process.
  - The system successfully searches all specified file types (.pdf, .docx, .doc) in the configured Google Workspace location.
  - A list of matching candidates is returned and displayed in the UI.

### 10.3. View candidate relevance score
- **ID**: US-003
- **Description**: As a Talent Consultant, I want to see a relevance score for each matched candidate so I can prioritize who to review first.
- **Acceptance criteria**:
  - Each candidate in the search results is displayed with a numerical or percentage-based score.
  - The search results are sorted by the relevance score in descending order by default.
  - The score accurately reflects the candidate's relevance to the provided job profile.

### 10.4. View AI-generated candidate summary
- **ID**: US-004
- **Description**: As a Talent Consultant, I want to read a short, AI-generated summary for each matched candidate so that I can understand their qualifications without reading their entire file.
- **Acceptance criteria**:
  - Each candidate in the search results includes a short summary (2-4 sentences).
  - The summary is generated by the AI based on the content of the candidate's documents.
  - The summary highlights the key skills and experiences that are most relevant to the job profile.

### 10.5. Ask simple questions via helpdesk
- **ID**: US-005
- **Description**: As a Talent Consultant, I want to ask a direct question about the candidate pool so I can get quick facts and figures.
- **Acceptance criteria**:
  - The UI has a text input field for helpdesk questions.
  - The system can parse and understand simple, natural language questions (e.g., "how many candidates know python?").
  - A direct, text-based answer is returned and displayed in the UI.

### 10.6. Handle no-result scenarios
- **ID**: US-006
- **Description**: As a user, when my search or query yields no results, I want to see a clear message so that I know the system has finished processing my request.
- **Acceptance criteria**:
  - If a candidate search returns zero matches, a "No relevant candidates found" message is displayed.
  - If a helpdesk query cannot be answered or finds no data, a "No results found for your query" message is displayed.
  - The message is user-friendly and clearly visible.

### 10.7. Secure data processing
- **ID**: US-007
- **Description**: As a System Administrator, I want to ensure the application processes data securely and in compliance with company policy to protect sensitive candidate information.
- **Acceptance criteria**:
  - The application does not store a persistent copy of candidate PII.
  - All data processing occurs within the approved secure environment (Google Cloud or local).
  - If external services are used, all PII is successfully anonymized before being sent.
  - The system passes all internal security and data compliance reviews.
