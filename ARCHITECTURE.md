# habit-forge-ai Architecture

## Overview
AI-powered personalized habit coaching app to build consistent habits and achieve your goals.

### Problem Statement
Individuals struggle with consistent habit tracking and goal attainment, often abandoning tracking apps due to tediousness and lack of personalized motivation. Existing solutions are too generic.

### Target Audience
Individuals aiming to improve personal productivity, health, and overall well-being through habit formation (students, professionals, fitness enthusiasts) who need personalized guidance.

### Core Entities


## Design System

### Typography
- **Font**: Roboto
- **Font Stack**: `'Roboto', sans-serif`
- **Google Fonts**: https://fonts.google.com/specimen/Roboto
- **Rationale**: Roboto offers excellent readability at various sizes, making it suitable for both headings and body text. Its clean and modern appearance aligns with a professional aesthetic. It also offers great accessibility support.

### Color Palette
A monochromatic palette with a strong accent color. Blue tones convey trust and reliability, while the orange accent provides energy and excitement.  Accessibility has been checked using online tools to ensure sufficient contrast.

- **Primary**: `#3F51B5` - Main brand color, used for headers and primary actions.  Indigo provides a sense of calm and professionalism.
- **Secondary**: `#7986CB` - Supporting color for secondary elements. A lighter shade of Indigo, providing visual hierarchy.
- **Accent**: `#FF5722` - Used for call-to-action buttons and highlights. Deep Orange is stimulating and energetic.
- **Neutral Text**: `#212121` - Primary text color for high contrast and readability. Almost Black provides clarity.
- **Neutral Background**: `#FAFAFA` - Main background color for content areas.  Light Grey provides comfortable contrast.
- **Neutral Border**: `#BDBDBD` - For card borders, dividers, and form inputs.  Grey provides a subtle separation.
- **Success**: `#4CAF50` - For success messages and confirmation. Green promotes positive affirmation.
- **Warning**: `#FF9800` - For warnings and non-critical alerts. Amber draws attention carefully.
- **Danger**: `#F44336` - For error messages and destructive actions. Red implies urgency.

## System Architecture

### 1. Core Components & Rationale
The application will be built using a layered architecture:

*   **Laravel (Backend):** Handles API endpoints, business logic, database interactions, and authentication.
*   **React/Inertia.js (Frontend):** Provides a dynamic and interactive user interface. Inertia.js simplifies routing and data passing between Laravel and React.
*   **MySQL (Database):** Stores user data, habits, messages, and other application-related information.
*   **Redis (Cache/Real-time):** Used for caching frequently accessed data and enabling real-time features like notifications and progress updates.
*   **Laravel Echo Server (Real-time Server):** Facilitates WebSocket connections for real-time communication.
*   **Typescript:** Will be used on both the frontend and backend (using packages like `ts-node`) to help enforce stricter typing to catch potential bugs.

Rationale: This stack provides a good balance of developer productivity, scalability, and maintainability. Laravel and React are popular frameworks with large communities and extensive documentation. Inertia.js bridges the gap between the two, allowing developers to build modern web applications with a more traditional server-side approach.

### 2. Database Schema Design
The database schema will include the following tables:

*   **users:**
    *   `id` (INT, primary key, auto-increment)
    *   `name` (VARCHAR)
    *   `email` (VARCHAR, unique)
    *   `password` (VARCHAR)
    *   `personality_assessment_results` (JSON, stores personality assessment data)
    *   `goal_setting_data` (JSON, stores user's goals and preferences)
    *   `created_at` (TIMESTAMP)
    *   `updated_at` (TIMESTAMP)

*   **habits:**
    *   `id` (INT, primary key, auto-increment)
    *   `user_id` (INT, foreign key referencing `users.id`)
    *   `name` (VARCHAR)
    *   `description` (TEXT)
    *   `frequency` (JSON, defines the habit's frequency, e.g., daily, weekly)
    *   `start_date` (DATE)
    *   `end_date` (DATE, nullable)
    *   `created_at` (TIMESTAMP)
    *   `updated_at` (TIMESTAMP)

*   **habit_entries:**
    *   `id` (INT, primary key, auto-increment)
    *   `habit_id` (INT, foreign key referencing `habits.id`)
    *   `date` (DATE)
    *   `completed` (BOOLEAN)
    *   `journal_entry` (TEXT, nullable)
    *   `data_source` (ENUM('manual', 'wearable'), default 'manual')
    *   `created_at` (TIMESTAMP)
    *   `updated_at` (TIMESTAMP)

*   **messages:**
    *   `id` (INT, primary key, auto-increment)
    *   `user_id` (INT, foreign key referencing `users.id`)
    *   `type` (ENUM('reminder', 'motivational'), default 'motivational')
    *   `content` (TEXT)
    *   `scheduled_at` (TIMESTAMP, nullable)
    *   `sent_at` (TIMESTAMP, nullable)
    *   `created_at` (TIMESTAMP)
    *   `updated_at` (TIMESTAMP)

*   **wearable_data:**
    *   `id` (INT, primary key, auto-increment)
    *   `user_id` (INT, foreign key referencing `users.id`)
    *   `timestamp` (TIMESTAMP)
    *   `data_type` (ENUM('steps', 'sleep', 'heart_rate'))
    *   `value` (FLOAT)

Relationships: One-to-many relationships between users and habits, habits and habit_entries, users and messages, and users and wearable_data. All foreign keys will have appropriate indexes for query optimization. Data validation will be implemented at the application level to ensure data integrity.

### 3. API Design & Key Endpoints
The API will follow RESTful principles and use JSON for data exchange. Key endpoints include:

*   **Authentication:**
    *   `POST /api/register` (Registers a new user)
    *   `POST /api/login` (Logs in an existing user)
    *   `POST /api/logout` (Logs out the current user)
    *   `GET /api/user` (Gets the current user's information)

*   **Habits:**
    *   `GET /api/habits` (Lists all habits for the current user)
    *   `POST /api/habits` (Creates a new habit)
    *   `GET /api/habits/{habit}` (Gets a specific habit)
    *   `PUT /api/habits/{habit}` (Updates a habit)
    *   `DELETE /api/habits/{habit}` (Deletes a habit)

*   **Habit Entries:**
    *   `GET /api/habits/{habit}/entries` (Lists all entries for a habit)
    *   `POST /api/habits/{habit}/entries` (Creates a new entry for a habit)
    *   `GET /api/habits/{habit}/entries/{entry}` (Gets a specific entry)
    *   `PUT /api/habits/{habit}/entries/{entry}` (Updates an entry)
    *   `DELETE /api/habits/{habit}/entries/{entry}` (Deletes an entry)

*   **Messages:**
    *   `GET /api/messages` (Lists all messages for the current user)
    *   `GET /api/messages/{message}` (Gets a specific message)

*   **Wearable Data:**
    *   `POST /api/wearable-data` (Uploads wearable data)

API Authentication: API authentication will be implemented using Laravel Sanctum, providing a simple and secure way to authenticate users using API tokens. Input validation will be performed using Laravel's built-in validation features to prevent malicious data from entering the system. Response codes will follow standard HTTP status codes.

### 4. Frontend Structure (TypeScript)
The frontend will be structured using a component-based architecture. Key components include:

*   **Layout Components:**
    *   `AppLayout.tsx`: The main layout component that wraps all other components.
    *   `Navbar.tsx`: The navigation bar.
    *   `Sidebar.tsx`: The sidebar (if needed).

*   **Page Components:**
    *   `Dashboard.tsx`: The main dashboard page.
    *   `HabitList.tsx`: A list of habits.
    *   `HabitDetail.tsx`: Details for a specific habit.
    *   `Profile.tsx`: User profile page.

*   **Reusable Components:**
    *   `Button.tsx`: A reusable button component.
    *   `Input.tsx`: A reusable input component.
    *   `Modal.tsx`: A reusable modal component.
    *   `ProgressBar.tsx`: A component to display progress visually.

State Management: For simple state management within components, we'll use React's built-in `useState` and `useContext` hooks. For more complex state management requirements (especially shared state across multiple components), Redux Toolkit or Zustand will be evaluated. However, to keep complexity minimal, we prefer context until more advanced state management becomes vital.

Typescript: All components and functions will be written in Typescript, ensuring type safety and improving code maintainability. Inertia.js will be used to pass data from the Laravel backend to the React frontend.  Utilities: a core `utils.ts` file will provide common functions. E.g. date formatting, string manipulation.

### 5. Real-time & Events Architecture
Real-time functionality will be implemented using Laravel Echo and Redis. The architecture will consist of the following components:

*   **Laravel Echo Server:** A Node.js server that acts as a WebSocket server.
*   **Redis:** A database used for broadcasting events.
*   **Laravel Events:** Laravel events will be used to trigger real-time updates.
*   **React Components:** React components will subscribe to specific channels using Laravel Echo and update their state when new events are received.

Example: When a user completes a habit, a `HabitCompleted` event will be fired from the Laravel backend. This event will be broadcast to a specific Redis channel. The React component that displays the user's progress will subscribe to this channel using Laravel Echo. When the event is received, the component will update its state and re-render, showing the updated progress.

Scaling: For scaling, the Redis instance can be clustered. Multiple Laravel Echo Server instances can be load balanced.  Web sockets can be sticky sessions to ensure that updates from the Laravel backend can be pushed to the right client.

### 6. Authentication & Authorization Flow
The authentication flow will be implemented using Laravel Sanctum. The flow will be as follows:

1.  **User Registration:** A new user registers by providing their name, email, and password. The password will be hashed using bcrypt.
2.  **User Login:** An existing user logs in by providing their email and password. Laravel Sanctum will generate an API token for the user.
3.  **API Authentication:** The user's API token will be included in the `Authorization` header of all API requests.
4.  **Authorization:** Laravel's built-in authorization features will be used to control access to resources. Policies will be defined to determine which users have access to which resources. Gates can be used for more granular authorization checks.

CSRF Protection: Laravel automatically provides CSRF protection for web routes.  For API routes, Laravel Sanctum provides CSRF protection by using API tokens.  Access Control Lists (ACLs) will be avoided unless necessary. Roles can be given to users. For more complex implementations, consider using a package like Spatie's laravel-permission.

### 7. Deployment & Scalability Plan
The application will be deployed using a containerized approach with Docker and Docker Compose.

*   **Infrastructure:** AWS (EC2, RDS, Elasticache (Redis), S3) or DigitalOcean (Droplets, Managed MySQL, Managed Redis, Spaces) will be used for hosting the application.
*   **Deployment Process:** A CI/CD pipeline will be set up using GitHub Actions or GitLab CI/CD to automate the deployment process.  The pipeline will build the Docker images, push them to a container registry (e.g., Docker Hub or AWS ECR), and deploy the containers to the server.
*   **Scalability:** The application will be designed to be horizontally scalable. Multiple instances of the Laravel application can be run behind a load balancer.  The database and Redis instances can be scaled independently as needed.  Queues can be implemented (using Laravel Queues and Redis) to handle long-running tasks asynchronously, preventing the web server from becoming overloaded. CDN (Content Delivery Network) should be used to serve the frontend's static assets. Caching (HTTP caching and Redis) should be aggressively applied to improve load times.
*   **Monitoring:** Datadog or New Relic will be used for monitoring the application's performance and health.  Alerts will be set up to notify the team of any issues.

### 8. Testing Strategy
A comprehensive testing strategy will be implemented to ensure the quality and reliability of the application:

*   **Backend Testing (Pest):**
    *   **Unit Tests:** To test individual units of code, such as models, controllers, and services.
    *   **Feature Tests:** To test the application's features, such as user registration, login, and habit tracking.
    *   **Integration Tests:** To test the interaction between different components of the application.

*   **Frontend Testing (Vitest & RTL):**
    *   **Unit Tests:** To test individual React components.
    *   **Integration Tests:** To test the interaction between different React components.
    *   **End-to-End Tests (Cypress or Playwright):** To test the entire application flow from the user's perspective.

Test Coverage: Code coverage metrics will be used to track the percentage of code that is covered by tests.  A minimum code coverage threshold will be set to ensure that all critical parts of the application are adequately tested.
Continuous Integration: Tests will be run automatically as part of the CI/CD pipeline to ensure that any new code changes do not introduce any regressions.  Mutation testing can be introduced later to measure the effectiveness of existing tests.

### 9. Security & Hardening Plan
A security-first approach will be adopted to protect the application from OWASP Top 10 vulnerabilities:

*   **Input Validation:** All user input will be validated to prevent injection attacks (SQL injection, XSS). Laravel's built-in validation features will be used for input validation.
*   **Output Encoding:** All data displayed to the user will be properly encoded to prevent XSS attacks. React automatically escapes values, but using `dangerouslySetInnerHTML` should be avoided without stringent sanitization.
*   **Authentication & Authorization:** Strong authentication and authorization mechanisms will be implemented to protect sensitive data.  Laravel Sanctum will be used for API authentication.  Policies and Gates will be used for authorization.
*   **Password Hashing:** Passwords will be hashed using bcrypt with a high cost factor.
*   **CSRF Protection:** Laravel's built-in CSRF protection will be enabled.
*   **Rate Limiting:** Rate limiting will be implemented to prevent brute-force attacks.
*   **Security Headers:** Security headers (e.g., Content-Security-Policy, X-Frame-Options, X-XSS-Protection) will be configured to protect against various attacks.
*   **Regular Security Audits:** Regular security audits will be conducted to identify and fix any vulnerabilities.
*   **Dependency Management:** Dependencies will be regularly updated to patch any security vulnerabilities. Tools like Dependabot can be used to automate dependency updates. Tools like Snyk can scan for vulnerabilities in dependencies.
*   **Data Encryption:** Sensitive data (e.g., API keys, database credentials) will be encrypted at rest and in transit.  HTTPS will be used for all communication. Database encryption using transparent data encryption (TDE) in MySQL will be considered.

### 10. Logging & Observability
A robust logging and observability strategy will be implemented to monitor the application's health and performance:

*   **Logging:** All application logs will be written to the `stderr` channel, allowing them to be easily collected and analyzed by a central logging system.  Structured logging will be used to make it easier to search and filter logs. Laravel's Monolog integration will be used for logging.
*   **Error Tracking:** Sentry will be used to track errors and exceptions in the application.
*   **Metrics Monitoring:** Prometheus and Grafana will be used to monitor key metrics, such as CPU usage, memory usage, response time, and error rate.
*   **Application Performance Monitoring (APM):** Laravel Telescope (for local development) and New Relic/Datadog (for production) will be used for application performance monitoring.  APM tools provide insights into the performance of individual requests and database queries.
*   **Alerting:** Alerts will be set up to notify the team of any issues, such as high error rates, slow response times, or high CPU usage.

Log Rotation: Log rotation will be configured to prevent log files from growing too large.

## Feature Implementation Roadmap

### 1. User onboarding flow with personality assessment and goal setting.
Guide new users through a personalized onboarding experience involving a personality assessment (e.g., using a pre-built assessment library integrated with AI for dynamic questionnaire generation), followed by assisted goal setting based on the assessment results. Personality data will be used to tailor habit suggestions.

**Database Changes:**
- Add `personality_assessment_results` (JSON) and `goal_setting_data` (JSON) columns to the `users` table.

**Backend Components:**
- UserController (for registration and profile updates)
- PersonalityAssessmentService (for handling personality assessment logic)
- GoalSettingService (for assisting with goal setting)
- AuthenticationService (for handling authentication)

**Frontend Components:**
- OnboardingWizard.tsx (main onboarding component)
- PersonalityAssessmentForm.tsx (form for the personality assessment)
- GoalSettingForm.tsx (form for setting goals)
- ResultsDisplay.tsx (to show the results)

**API Endpoints:**
- POST /api/personality-assessment (submits the personality assessment)
- POST /api/goal-setting (submits the goal-setting form)
- GET /api/personality-assessment/questions (fetches questions for the assessment based on user input)

**Testing Requirements:**
- Feature test for user registration with personality assessment and goal setting
- Unit tests for PersonalityAssessmentService and GoalSettingService
- Component tests for OnboardingWizard.tsx and its sub-components


### 2. AI-powered personalized habit suggestions based on user data.
Use an AI model (e.g., a recommendation engine trained on user data and behavioral science principles) to provide personalized habit suggestions to users. The AI should consider user's personality, goals, and past activity.

**Backend Components:**
- HabitSuggestionService (AI-powered habit suggestion logic)
- HabitController (for retrieving habit suggestions)

**Frontend Components:**
- HabitSuggestions.tsx (displays personalized habit suggestions)
- HabitCard.tsx (displays individual habit suggestions)

**API Endpoints:**
- GET /api/habit-suggestions (returns personalized habit suggestions)

**Testing Requirements:**
- Feature test for retrieving personalized habit suggestions
- Unit tests for HabitSuggestionService
- Component tests for HabitSuggestions.tsx and HabitCard.tsx


### 3. Habit tracking interface with visual progress indicators and journal entries.
Create an intuitive habit tracking interface that allows users to easily track their progress, view visual representations of their achievements, and add journal entries for each habit.

**Database Changes:**
- No database schema changes.  Using `habit_entries` table.

**Backend Components:**
- HabitEntryController (for creating, reading, updating, and deleting habit entries)
- HabitService (for fetching habits and related data)

**Frontend Components:**
- HabitTracker.tsx (main habit tracking component)
- HabitEntryForm.tsx (form for creating habit entries)
- HabitProgress.tsx (displays visual progress indicators)
- JournalEntry.tsx (a single journal entry)

**API Endpoints:**
- POST /api/habits/{habit}/entries (creates a new habit entry)
- GET /api/habits/{habit}/entries/{date} (gets a specific habit entry by date)
- PUT /api/habits/{habit}/entries/{entry} (updates a habit entry)

**Testing Requirements:**
- Feature tests for creating, reading, updating, and deleting habit entries
- Component tests for HabitTracker.tsx, HabitEntryForm.tsx, and HabitProgress.tsx


### 4. Personalized reminders and motivational messages triggered by AI.
Implement AI-driven personalized reminders and motivational messages to encourage users and help them stay on track.  The AI model should be trained on behavioral science principles.

**Backend Components:**
- MessageScheduler (schedules reminders and motivational messages)
- MotivationalMessageService (AI-powered message generation)
- MessageController (for managing messages)

**Real-time Events:**
- MessageReceived (broadcasted when a new message is available for a user)

**Testing Requirements:**
- Feature tests for scheduling and sending messages
- Unit tests for MotivationalMessageService
- Integration tests for MessageScheduler and MotivationalMessageService


### 5. Wearable device integration for automatic data logging and analysis.
Integrate with wearable devices (e.g., Fitbit, Apple Watch) to automatically log habit-related data (e.g., steps, sleep). Analyze this data to provide insights and personalized recommendations.

**Database Changes:**
- Create a `wearable_data` table to store wearable device data.

**Backend Components:**
- WearableDataService (for processing and analyzing wearable data)
- WearableDataController (for receiving data from wearable devices)

**Frontend Components:**
- WearableDataIntegration.tsx (component to manage wearable device integration)
- WearableDataDisplay.tsx (component to display analyzed data)

**API Endpoints:**
- POST /api/wearable-data (receives data from wearable devices)

**Testing Requirements:**
- Feature tests for receiving and processing wearable data
- Unit tests for WearableDataService
- Component tests for WearableDataIntegration.tsx and WearableDataDisplay.tsx


## Metadata
- **Generated**: 2025-06-21T19:18:15.815799+00:00
- **Project Type**: Laravel React Starter Kit
- **Architecture Version**: 1.0.0

---
*This architecture document is maintained by the CodeWebMobile AI system and should be the source of truth for all development decisions.*
