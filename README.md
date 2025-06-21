```markdown
# Habit Forge AI: Your AI-Powered Personalized Habit Coach

## About Habit Forge AI

Habit Forge AI is a personalized habit coaching application designed to help individuals build consistent habits and achieve their goals by leveraging the power of Artificial Intelligence. It provides a tailored approach to habit formation, addressing the common pitfalls of generic habit trackers and offering customized support for users on their journey to self-improvement.

## Problem Statement

Individuals frequently struggle with consistent habit tracking and goal attainment. Many abandon habit tracking apps due to tediousness and a lack of personalized motivation. Existing solutions are often too generic, failing to cater to individual needs, preferences, and personality traits. This results in a high rate of attrition and a lack of sustainable habit formation.

## Target Audience and Value Proposition

**Target Audience:**

*   Individuals aiming to improve personal productivity.
*   Individuals striving to enhance their health and overall well-being through habit formation.
*   Students, professionals, and fitness enthusiasts who require personalized guidance and motivation to build and maintain positive habits.

**Value Proposition:**

Habit Forge AI provides a personalized habit coaching experience, adapting to each user's unique needs and goals. It offers:

*   **Personalized guidance:** AI-driven insights and suggestions tailored to individual personality traits and goals.
*   **Increased motivation:** Personalized reminders and motivational messages to keep users engaged.
*   **Seamless tracking:** Integration with wearable devices for effortless data logging and analysis.
*   **Sustainable habits:** A comprehensive system designed to foster long-term habit formation.

## Key Features

Habit Forge AI's initial features include:

*   **User Onboarding Flow with Personality Assessment and Goal Setting:** A guided onboarding experience that assesses users' personalities and helps them define clear, achievable goals.
*   **AI-Powered Personalized Habit Suggestions:** Intelligent habit suggestions based on user data, personality, and goals.
*   **Habit Tracking Interface:** An intuitive interface with visual progress indicators and journal entries for tracking habits.
*   **Personalized Reminders and Motivational Messages:** AI-triggered reminders and motivational messages to keep users on track.
*   **Wearable Device Integration:** Automatic data logging and analysis through integration with wearable devices like Fitbit and Apple Health.

## Tech Stack

*   **Backend:** Laravel 11+ (PHP Framework)
*   **Frontend:** React 18+ with TypeScript
*   **Database:** MySQL (NOT PostgreSQL)
*   **Cache/Queue:** Redis
*   **Real-time:** Laravel Echo Server
*   **AI/ML:** Python with TensorFlow/PyTorch (for model training and inference - implemented via API integration)

## Architecture Overview

The application follows a layered architecture, comprising the following key components:

*   **Frontend (React/TypeScript):**  Handles user interaction, data presentation, and communication with the backend API.
*   **Backend (Laravel):**  Manages business logic, API endpoints, data validation, authentication, and database interactions.
*   **Database (MySQL):** Stores persistent data, including user profiles, habits, logs, and messages.
*   **Cache (Redis):** Caches frequently accessed data to improve performance and enables real-time functionality.
*   **Real-time Server (Laravel Echo Server):**  Enables real-time communication between the backend and frontend using WebSockets.
*   **AI Model (Python/TensorFlow/PyTorch):**  Provides personalized habit suggestions, motivational messages, and data analysis.  Accessed via API endpoints.

See the diagram below for a conceptual overview:

```
+---------------------+      +---------------------+      +---------------------+
|   React Frontend    | <--> |   Laravel Backend   | <--> |     MySQL Database    |
|   (TypeScript)      |      |   (PHP)             |      |                       |
+---------------------+      +---------------------+      +---------------------+
         ^                         ^                         |
         |                         |                         |
         |     Laravel Echo Server |        Redis          |
         |                         |                         |
         +-------------------------+      +---------------------+
                                           |     AI Model       |
                                           | (Python/TF/PyTorch) |
                                           +---------------------+
```

This architecture allows for scalability, maintainability, and future expansion.

## Prerequisites

Before setting up the development environment, ensure you have the following installed:

*   **PHP:** Version 8.2 or higher
*   **Composer:**  PHP dependency manager
*   **Node.js:** Version 18+
*   **npm:** Node.js package manager
*   **MySQL:** Version 8.0 or higher
*   **Redis:**  For caching and real-time features
*   **Laravel CLI:**  For creating and managing Laravel projects
*   **Git:** For version control

## Installation Steps

1.  **Clone the repository:**

    ```bash
    git clone <repository_url>
    cd habit-forge-ai
    ```

2.  **Install Laravel dependencies:**

    ```bash
    composer install
    ```

3.  **Install frontend dependencies:**

    ```bash
    cd client  # Or the directory where React code resides. Adjust if different
    npm install
    ```

4.  **Set up environment variables (see `.env.example`):**
    * Copy `.env.example` to `.env` and adjust the values.

    ```bash
    cp .env.example .env
    ```

5.  **Generate Laravel application key:**

    ```bash
    php artisan key:generate
    ```

6.  **Configure the database:**

    *   Update the `.env` file with your MySQL database credentials.

    ```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=habit_forge
    DB_USERNAME=your_mysql_username
    DB_PASSWORD=your_mysql_password
    ```

7.  **Run database migrations and seeders:**

    ```bash
    php artisan migrate --seed
    ```

8.  **Start the Laravel development server:**

    ```bash
    php artisan serve
    ```

9.  **Start the React development server:**

    ```bash
    cd client
    npm start
    ```

## Environment Setup

The application requires the following environment variables:

*   `APP_NAME`: The name of the application.
*   `APP_ENV`: The environment (local, production, etc.).
*   `APP_KEY`: The application key (generated by `php artisan key:generate`).
*   `APP_DEBUG`: Enable or disable debugging mode (set to `true` for local development).
*   `APP_URL`: The application URL.
*   `DB_CONNECTION`: The database connection (set to `mysql`).
*   `DB_HOST`: The database host.
*   `DB_PORT`: The database port.
*   `DB_DATABASE`: The database name.
*   `DB_USERNAME`: The database username.
*   `DB_PASSWORD`: The database password.
*   `REDIS_HOST`: The Redis host.
*   `REDIS_PORT`: The Redis port.
*   `REDIS_PASSWORD`: The Redis password (if any).
*   `BROADCAST_DRIVER`: Set to `redis` for Laravel Echo.
*   `CACHE_DRIVER`: Set to `redis`.
*   `QUEUE_CONNECTION`: Set to `redis`.
*   `SESSION_DRIVER`: Set to `redis`.
*   `WEARABLE_API_KEY_FITBIT`: API key for Fitbit integration.
*   `WEARABLE_API_KEY_APPLE_HEALTH`: API key for Apple Health integration.
*   `AI_MODEL_ENDPOINT`: The URL endpoint for the AI model service (e.g., for habit suggestions).

Example `.env`:

```
APP_NAME=HabitForgeAI
APP_ENV=local
APP_KEY=base64:your_secret_key
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=habit_forge
DB_USERNAME=root
DB_PASSWORD=

REDIS_HOST=127.0.0.1
REDIS_PORT=6379
REDIS_PASSWORD=null

BROADCAST_DRIVER=redis
CACHE_DRIVER=redis
QUEUE_CONNECTION=redis
SESSION_DRIVER=redis

WEARABLE_API_KEY_FITBIT=your_fitbit_api_key
WEARABLE_API_KEY_APPLE_HEALTH=your_apple_health_api_key
AI_MODEL_ENDPOINT=http://localhost:5000/api/habitsuggestions
```

## Development Workflow

1.  **Create a feature branch:**  `git checkout -b feature/your-feature-name`
2.  **Implement your changes:**  Follow the established coding standards and architecture guidelines.
3.  **Write tests:**  Ensure your changes are thoroughly tested.  Run backend tests with `php artisan test` and frontend tests with `npm test`.
4.  **Commit your changes:**  Use clear and concise commit messages.
5.  **Push your branch to the remote repository:**  `git push origin feature/your-feature-name`
6.  **Create a pull request:**  Submit a pull request to the `main` branch.

## Testing

*   **Backend (Laravel):**  Use Pest for unit, feature, and integration tests. Run tests with `php artisan test`.
*   **Frontend (React):** Use Vitest and React Testing Library (RTL) for unit and component tests. Run tests with `npm test`.
*   **End-to-End Tests:** Use Cypress or Playwright for E2E testing (setup instructions to be added).

Aim for high code coverage to ensure the reliability of the application. Implement continuous integration to run tests automatically on every code push.

## Database Schema

The database schema includes the following tables:

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

*   **ai_suggestions:**
    *   `id` (INT, primary key, auto-increment)
    *   `user_id` (INT, foreign key referencing `users.id`)
    *   `habit_id` (INT, foreign key referencing `habits.id`)
    *   `suggestion_text` (TEXT)
    *   `timestamp` (TIMESTAMP)

*   **challenges:**
    *   `id` (INT, primary key, auto-increment)
    *   `name` (VARCHAR)
    *   `description` (TEXT)
    *   `start_date` (DATE)
    *   `end_date` (DATE)
    *   `reward` (TEXT)

## Deployment

The application will be deployed using a containerized approach with Docker and Docker Compose.

1.  **Dockerize the application:**  Create Dockerfiles for the frontend and backend.
2.  **Create a Docker Compose file:**  Define the services, networks, and volumes.
3.  **Build the Docker images:**  `docker-compose build`
4.  **Push the images to a container registry:**  (e.g., Docker Hub, AWS ECR)
5.  **Deploy the containers to a cloud provider:**  (e.g., AWS, DigitalOcean) or a server.
6.  **Set up a CI/CD pipeline:**  Automate the deployment process using GitHub Actions or GitLab CI/CD.

A more detailed deployment guide will be added.

## Monetization Strategy

Habit Forge AI utilizes a freemium model:

*   **Free Tier:** Basic habit tracking functionality with limited features.
*   **Premium Tier:** Unlocks personalized AI coaching, advanced analytics, habit challenges, wearable device integration, and priority support.

Premium subscriptions will be offered on a recurring monthly or annual basis.

## Contributing Guidelines

We welcome contributions to Habit Forge AI! Please follow these guidelines:

1.  **Fork the repository.**
2.  **Create a feature branch:** `git checkout -b feature/your-feature`
3.  **Implement your changes:** Follow the coding standards and architecture guidelines.
4.  **Write tests:** Ensure your changes are thoroughly tested.
5.  **Commit your changes:** Use clear and concise commit messages.
6.  **Push your branch to your forked repository:** `git push origin feature/your-feature`
7.  **Create a pull request:** Submit a pull request to the `main` branch.

## Design System

*   **Font:** Roboto (`'Roboto', sans-serif`) - [Google Fonts](https://fonts.google.com/specimen/Roboto)
*   **Color Palette:**

    | Color             | HEX     | Usage                                                        |
    | ----------------- | ------- | ------------------------------------------------------------ |
    | Primary           | #3F51B5 | Main brand color, headers, primary actions                     |
    | Secondary         | #7986CB | Supporting color, secondary elements                         |
    | Accent            | #FF5722 | Call-to-action buttons, highlights                           |
    | Neutral Text      | #212121 | Primary text color                                           |
    | Neutral Background | #FAFAFA | Main background color                                        |
    | Neutral Border    | #BDBDBD | Card borders, dividers, form inputs                          |
    | Success           | #4CAF50 | Success messages and confirmation                             |
    | Warning           | #FF9800 | Warnings and non-critical alerts                              |
    | Danger            | #F44336 | Error messages and destructive actions                        |

## Feature Implementation Roadmap

| Feature                                                                     | Description                                                                                                                                                                                                                                                                                                          | Required DB Changes                                                                              | Impacted Backend Components                                                                                                                                  | Impacted Frontend Components                                                                                                                                       | New API Endpoints                                                                                                                                                                                                | Real-time Events | Suggested Tests                                                                                                                                                                                                                  |
| --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| User onboarding flow with personality assessment and goal setting.            | Guide new users through a personalized onboarding experience involving a personality assessment, followed by assisted goal setting based on the assessment results.                                                                                                                                                 | Add `personality_assessment_results` (JSON) and `goal_setting_data` (JSON) columns to `users` table. | UserController, PersonalityAssessmentService, GoalSettingService, AuthenticationService                                                                | OnboardingWizard.tsx, PersonalityAssessmentForm.tsx, GoalSettingForm.tsx, ResultsDisplay.tsx                                                                     | POST /api/personality-assessment, POST /api/goal-setting, GET /api/personality-assessment/questions                                                                                                                |                  | Feature test for user registration with personality assessment and goal setting, Unit tests for PersonalityAssessmentService and GoalSettingService, Component tests for OnboardingWizard.tsx and sub-components |
| AI-powered personalized habit suggestions based on user data.                | Use an AI model to provide personalized habit suggestions to users. The AI should consider user's personality, goals, and past activity.                                                                                                                                                                             | None                                                                                             | HabitSuggestionService, HabitController                                                                                                                       | HabitSuggestions.tsx, HabitCard.tsx                                                                                                                                | GET /api/habit-suggestions                                                                                                                                                                                            |                  | Feature test for retrieving personalized habit suggestions, Unit tests for HabitSuggestionService, Component tests for HabitSuggestions.tsx and HabitCard.tsx                                                  |
| Habit tracking interface with visual progress indicators and journal entries. | Create an intuitive habit tracking interface that allows users to easily track their progress, view visual representations of their achievements, and add journal entries for each habit.                                                                                                                            | None                                                                                             | HabitEntryController, HabitService                                                                                                                          | HabitTracker.tsx, HabitEntryForm.tsx, HabitProgress.tsx, JournalEntry.tsx                                                                                          | POST /api/habits/{habit}/entries, GET /api/habits/{habit}/entries/{date}, PUT /api/habits/{habit}/entries/{entry}                                                                                                  |                  | Feature tests for creating, reading, updating, and deleting habit entries, Component tests for HabitTracker.tsx, HabitEntryForm.tsx, and HabitProgress.tsx                                                      |
| Personalized reminders and motivational messages triggered by AI.          | Implement AI-driven personalized reminders and motivational messages to encourage users and help them stay on track.                                                                                                                                                                                                 | None                                                                                             | MessageScheduler, MotivationalMessageService, MessageController                                                                                               | None                                                                                                                                                                 | None                                                                                                                                                                                                           | MessageReceived  | Feature tests for scheduling and sending messages, Unit tests for MotivationalMessageService, Integration tests for MessageScheduler and MotivationalMessageService                                                 |
| Wearable device integration for automatic data logging and analysis.          | Integrate with wearable devices to automatically log habit-related data (e.g., steps, sleep). Analyze this data to provide insights and personalized recommendations.                                                                                                                                              | Create `wearable_data` table.                                                                  | WearableDataService, WearableDataController                                                                                                                   | WearableDataIntegration.tsx, WearableDataDisplay.tsx                                                                                                            | POST /api/wearable-data                                                                                                                                                                                            |                  | Feature tests for receiving and processing wearable data, Unit tests for WearableDataService, Component tests for WearableDataIntegration.tsx and WearableDataDisplay.tsx                                       |
