
# Auto Hue - Car Spotter's Log

Auto Hue is a fun application designed for car enthusiasts to log the vehicles they spot, noting their brand, model, and color. It features an AI-powered persona generator that creates a unique personality for each car combination, adding a creative twist to your car spotting adventures!

## Features

*   **Car Logging**: Easily select car brand, model, and color from predefined lists.
*   **Game Sessions**: Track your car spots within specific game numbers/sessions.
    *   Generate random game numbers.
    *   Enter existing game numbers (with a warning if the ID might be in use).
*   **AI Car Persona**: For each unique car (brand, model, color), get a creatively generated persona powered by Gemini AI. Personas are designed to be varied even for the same car input.
*   **Firestore Integration**: Saves your spotted cars to a Firebase Firestore database, organized by game number.
*   **Statistics Page**: View statistics for your car spotting sessions, including:
    *   Total cars spotted per game.
    *   Breakdowns by color, brand, and model.
    *   A detailed table comparing colors across different car models.
*   **User-Friendly Interface**: Built with Next.js and ShadCN UI components for a clean and responsive experience.

## Tech Stack

*   **Framework**: Next.js (App Router)
*   **Language**: TypeScript
*   **Styling**: Tailwind CSS
*   **UI Components**: ShadCN UI
*   **AI**: Google Gemini (via Genkit)
*   **Database**: Firebase Firestore
*   **Deployment**: Firebase App Hosting

## Getting Started

### Prerequisites

*   Node.js (v18 or later recommended)
*   npm or yarn
*   Firebase Account
*   Firebase CLI (`npm install -g firebase-tools`)

### Setup

1.  **Clone the Repository**:
    ```bash
    git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git
    cd YOUR_REPOSITORY_NAME
    ```

2.  **Install Dependencies**:
    ```bash
    npm install
    # or
    # yarn install
    ```

3.  **Firebase Project Setup**:
    *   Create a new Firebase project at [https://console.firebase.google.com/](https://console.firebase.google.com/).
    *   Enable **Firestore Database**. When prompted, start in "test mode" for easy setup, or "production mode" and configure security rules (see step 5).
    *   Register a Web App in your Firebase project settings.
    *   Note down the Firebase configuration object.

4.  **Environment Variables**:
    *   Create a `.env` file in the root of the project by copying `.env.example` (if one exists) or creating it from scratch.
    *   Populate it with your Firebase project credentials:
        ```env
        NEXT_PUBLIC_FIREBASE_API_KEY="YOUR_API_KEY"
        NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN="YOUR_AUTH_DOMAIN"
        NEXT_PUBLIC_FIREBASE_PROJECT_ID="YOUR_PROJECT_ID"
        NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET="YOUR_STORAGE_BUCKET"
        NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID="YOUR_MESSAGING_SENDER_ID"
        NEXT_PUBLIC_FIREBASE_APP_ID="YOUR_APP_ID"
        NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID="YOUR_MEASUREMENT_ID" # Optional
        ```
    *   You will also need to set up a Google AI (Gemini) API key for Genkit. If you have one, you can set it up as per Genkit's documentation, potentially in your cloud environment or locally. For local development, ensure Genkit can access your Gemini credentials.

5.  **Firestore Security Rules**:
    *   In your Firebase project's Firestore Database section, go to the "Rules" tab.
    *   Update the rules to allow access to the `spottedCars<GAME_ID>` collections and the `game_sessions` collection. For development, you can use:
        ```firestore-rules
        rules_version = '2';
        service cloud.firestore {
          match /databases/{database}/documents {
            match /{collectionId}/{document=**} {
              allow read, write: if (
                collectionId.matches('^spottedCars.*') ||
                collectionId == 'game_sessions'
              );
            }
          }
        }
        ```
        **Note**: For production, replace `if (...)` with more secure rules, typically involving `request.auth != null;`.

### Running Locally

1.  **Start the Genkit development server (for AI features)**:
    *   In one terminal, run:
        ```bash
        npm run genkit:watch
        ```

2.  **Start the Next.js Development Server**:
    *   In another terminal, run:
        ```bash
        npm run dev
        ```
    *   Open your browser and navigate to `http://localhost:9002` (or the port specified in your `package.json`).

## Deployment

This project is configured for **Firebase App Hosting**.

1.  **Connect GitHub to Firebase App Hosting**:
    *   In the Firebase Console, go to "App Hosting".
    *   Create a new backend or link an existing one to your GitHub repository.
    *   Select your production branch (e.g., `main` or `production`).
2.  **Configure Environment Variables in App Hosting**:
    *   In your App Hosting backend settings in the Firebase console, add all the `NEXT_PUBLIC_FIREBASE_...` environment variables from your `.env` file.
    *   Also, ensure your Gemini API key is securely configured for the App Hosting environment.
3.  **Push to GitHub**:
    *   Commit your changes and push them to the production branch linked to App Hosting.
        ```bash
        git add .
        git commit -m "Deployment commit"
        git push origin your-production-branch
        ```
    *   Firebase App Hosting will automatically build and deploy your application. Monitor the status in the Firebase console.

## Project Structure

*   `src/app/`: Next.js App Router pages and layouts.
    *   `page.tsx`: Main car spotting/logging page.
    *   `stats/page.tsx`: Statistics display page.
    *   `actions/`: Server Actions for database operations and session management.
*   `src/components/`: Reusable React components.
    *   `auto-hue/`: Components specific to the car logging features.
    *   `ui/`: ShadCN UI components.
*   `src/lib/`: Utility functions, Firebase setup (`firebase.ts`), and static data (`car-data.ts`).
*   `src/ai/`: Genkit AI integration.
    *   `flows/generate-car-persona.ts`: The Genkit flow for generating car personas.
    *   `genkit.ts`: Genkit configuration.
*   `public/`: Static assets.
*   `apphosting.yaml`: Configuration for Firebase App Hosting.

## Contributing

Feel free to fork this repository, make improvements, and submit pull requests!

## License

This project is open-source. (Consider adding a specific license like MIT if you wish).

---

Happy Car Spotting!
