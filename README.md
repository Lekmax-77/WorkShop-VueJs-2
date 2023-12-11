# Vue.js Workshop Part 2: Building a TODO List Application with Vuetify

## Introduction

Welcome to Part 2 of our Vue.js Workshop! In this session, you'll develop a TODO list application using Vue.js and Vuetify. You'll learn how to add user authentication features, interact with a Node.js backend, and utilize OAuth2 for Google authentication.

### Workshop Objectives

- Develop a user-friendly interface for a TODO list application.
- Implement user registration and login functionalities.
- Connect the application to a Node.js backend.
- Implement OAuth2 authentication with Google.

### Duration

- 2 hours and 30 minutes

### Agenda

- **Introduction and Setup**: 20 minutes
- **Step 1 - User Authentication and Registration**: 30 minutes
- **Step 2 - TODO List Interface with Vuetify**: 40 minutes
- **Step 3 - Integration with Node.js Backend**: 30 minutes
- **Step 4 - OAuth2 Authentication with Google**: 25 minutes
- **Q&A**: 5 minutes

## Initial Setup

The initial setup of the workshop will guide participants through creating a Vue.js application using Vite.js, a modern frontend build tool, and setting up Vuetify for UI components. This setup also includes launching a Node.js backend server.

### Step 1: Installing Dependencies

1. **Install Node.js**: Ensure Node.js is installed on all participants' machines. It can be downloaded from the [Node.js official website](https://nodejs.org/).
2. **Install Vite.js**: Vite.js will be used to create the Vue.js project. It can be installed globally using npm:
    
    ```bash
    npm init vite@latest
    
    ```
    
3. **Create a Vue.js Project with Vite.js**: Set up a new Vue.js project using Vite.js. In the terminal, run the following commands:
    
    ```bash
    npm init vite@latest todo-list-app -- --template vue
    cd todo-list-app
    ```
    
4. **Add Vuetify**: To integrate Vuetify, you need to install it and its dependencies. Run the following commands inside your project directory:
    
    ```bash
    npm install vuetify@next
    npm install sass@1.32.12
    ```
    
    Then, set up Vuetify in your main Vue file (e.g., `main.js`):
    
    ```jsx
    import { createApp } from 'vue'
    import { createVuetify } from 'vuetify'
    import App from './App.vue'
    import 'vuetify/styles' // Global CSS has to be imported
    
    const app = createApp(App)
    
    // Ensure to use the 'createVuetify' function
    const vuetify = createVuetify()
    
    app.use(vuetify)
    app.mount('#app')
    ```
    

### Step 2: Launching Node.js Backend

1. **Download or Clone the Backend Repository**: Participants should clone the provided backend repository using Git or download it as a ZIP file and extract it.
2. **Install Backend Dependencies**: Navigate to the backend project directory in the terminal, and install necessary packages:
    
    ```bash
    npm install
    ```
    
3. **Start the Backend Server**: Run the server using:
    
    ```bash
    npm run dev
    ```
    
    This will launch the backend server on `http://localhost:3000`, ready to respond to the Vue.js application's requests.
    

### Troubleshooting Tips

- **Verify Node.js Installation**: Use `node --version` to check if Node.js is installed.
- **Check Vite.js and Vuetify Setup**: Ensure that Vite.js and Vuetify are correctly set up in your project.
- **Backend Server Issues**: If there are issues starting the backend server, check for error messages in the terminal and resolve them accordingly.

## Step 1: User Authentication and Registration

### Objective

In this step, participants will create a registration form and a login form using Vuetify components. They will also learn to connect these forms to the backend to handle user authentication and registration.

### Creating the Registration Form

1. **Setup the Registration Form Component**: Create a new file named `Register.vue` in the `src/components/` directory. This component will contain the registration form.
2. **Implement the Form with Vuetify Components**: Use Vuetify's form components to build the registration form. Here's an example:
    
    ```
    <template>
      <v-container>
        <v-form>
          <v-text-field label="Email" v-model="email" type="email"></v-text-field>
          <v-text-field label="Password" v-model="password" type="password"></v-text-field>
          <v-btn @click="register">Register</v-btn>
        </v-form>
      </v-container>
    </template>
    
    <script>
    export default {
      data() {
        return {
          email: '',
          password: ''
        };
      },
      methods: {
        async register() {
          // API call to backend for registration
          // Handle response
        }
      }
    };
    </script>
    
    ```
    

### Creating the Login Form

1. **Setup the Login Form Component**: Similarly, create a `Login.vue` file in the `src/components/` directory.
2. **Build the Form with Vuetify**: Use Vuetify components to create the login interface.
    
    ```
    <template>
      <v-container>
        <v-form>
          <v-text-field label="Email" v-model="email" type="email"></v-text-field>
          <v-text-field label="Password" v-model="password" type="password"></v-text-field>
          <v-btn @click="login">Login</v-btn>
        </v-form>
      </v-container>
    </template>
    
    <script>
    export default {
      data() {
        return {
          email: '',
          password: ''
        };
      },
      methods: {
        async login() {
          // API call to backend for login
          // Handle response and store token
        }
      }
    };
    </script>
    
    ```
    

### Connecting to the Backend

1. **Handling Registration**: In the `register` method of the `Register.vue` component, make an HTTP POST request to your backend endpoint (e.g., `http://localhost:3000/register`) with the user's email and password.
2. **Handling Login**: Similarly, in the `login` method of the `Login.vue` component, make a POST request to the backend (e.g., `http://localhost:3000/login`). On successful login, the backend should return a token which can be stored locally (e.g., in localStorage) for session management.

### Tips for Participants

- **Validation**: Implement form validation using Vuetify's built-in validation rules or create custom validations.
- **Error Handling**: Properly handle any errors returned from the backend, such as displaying error messages to the user.
- **Security**: Remind participants not to store sensitive data directly in the frontend and to handle authentication tokens securely.



## Step 2: TODO List Interface with Vuetify

### Objective

In this step, participants will develop a TODO list interface where users can add, display, and delete tasks. This interface will be built using Vuetify components and will interact with the backend to persist tasks.

### Creating the TODO List Component

1. **Setup the TODO List Component**: Create a new file named `TodoList.vue` in the `src/components/` directory. This component will contain the list and the form to add new tasks.
2. **Building the Interface with Vuetify**:
    
    Use various Vuetify components to create a user-friendly TODO list interface. Here’s an example implementation:
    
    ```
    <template>
      <v-container>
        <v-form @submit.prevent="addTask">
          <v-text-field label="New Task" v-model="newTask" :rules="[rules.required]"></v-text-field>
          <v-btn type="submit" color="primary">Add Task</v-btn>
        </v-form>
    
        <v-list dense>
          <v-list-item
            v-for="(task, index) in tasks"
            :key="index"
          >
            <v-list-item-content>{{ task.text }}</v-list-item-content>
            <v-list-item-action>
              <v-btn icon @click="deleteTask(task.id)">
                <v-icon>mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </v-container>
    </template>
    
    <script>
    export default {
      data() {
        return {
          newTask: '',
          tasks: [], // This will hold the tasks fetched from the backend
          rules: {
            required: value => !!value || 'Required.'
          }
        };
      },
      methods: {
        addTask() {
          // API call to backend to add a task
          // Then fetch updated tasks list
        },
        deleteTask(taskId) {
          // API call to backend to delete a task
          // Then fetch updated tasks list
        },
        fetchTasks() {
          // API call to backend to get tasks
        }
      },
      mounted() {
        this.fetchTasks();
      }
    };
    </script>
    
    ```
    

### Connecting to the Backend

1. **Adding Tasks**: In the `addTask` method, make an HTTP POST request to the backend (e.g., `http://localhost:3000/tasks`) with the new task's details. After adding, fetch the updated list of tasks.
2. **Fetching Tasks**: Use an HTTP GET request in the `fetchTasks` method to retrieve the current list of tasks from the backend.
3. **Deleting Tasks**: In the `deleteTask` method, make an HTTP DELETE request to the backend (e.g., `http://localhost:3000/tasks/{taskId}`) to remove a task. Then, update the list to reflect the changes.

### Tips for Participants

- **Task Management**: Encourage participants to think about how they can expand the functionality, like editing tasks or marking them as completed.
- **Responsive Design**: Use Vuetify's grid system to make the TODO list responsive.
- **User Feedback**: Implement loading indicators and messages to enhance user experience during API calls.



## Step 3: Integration with Node.js Backend

### Objective

In this step, participants will learn how to connect their Vue.js application with a Node.js backend. This includes making HTTP requests to handle CRUD operations for the TODO list and user authentication.

### Setting Up Axios for HTTP Requests

1. **Install Axios**: First, install Axios, a promise-based HTTP client, which will be used to make requests to the backend.
    
    ```bash
    npm install axios
    
    ```
    
2. **Configure Axios**: In your Vue.js project, set up Axios. You can create a dedicated file for Axios configuration (e.g., `axios-config.js`) or configure it directly in your main app file.
    
    ```jsx
    import axios from 'axios';
    
    axios.defaults.baseURL = '<http://localhost:3000>'; // Your backend base URL
    
    ```
    

### Making Requests to the Backend

1. **User Authentication**: Send requests to your backend endpoints for user registration and login. Handle responses to maintain user sessions.
    
    ```jsx
    // Example for a login request
    axios.post('/login', { email: this.email, password: this.password })
      .then(response => {
        // Handle successful login
        // Possibly store the auth token
      })
      .catch(error => {
        // Handle errors
      });
    
    ```
    
2. **CRUD Operations for TODO List**: Implement functions to create, read, update, and delete TODO items using Axios to make requests to your Node.js backend.
    
    ```jsx
    // Example for fetching TODO items
    axios.get('/todos')
      .then(response => {
        // Handle response data
        this.todos = response.data;
      })
      .catch(error => {
        // Handle errors
      });
    
    ```
    

### Error Handling and Feedback

1. **Error Handling**: Implement proper error handling for all Axios requests to catch and display any errors that might occur during API calls.
2. **User Feedback**: Provide feedback to the user during data operations, such as loading indicators when waiting for a response or success/error messages upon completion.

### Tips for Participants

- **Centralize API Calls**: Consider creating a centralized service or using Vuex for managing all Axios calls.
- **Token Management**: For authenticated requests, manage auth tokens securely. Use interceptors in Axios to automatically attach tokens to requests.
- **Environment Variables**: Use environment variables for API URLs and other sensitive data.



## Step 4: OAuth2 Authentication with Google

### Objective

In this step, participants will learn to integrate Google OAuth2 into their Vue.js application, enabling users to authenticate using their Google accounts. This process involves setting up the Google OAuth2 client, handling the authentication flow, and communicating with the backend.

### Setting Up Google OAuth2

1. **Create a Google Cloud Project**: Direct participants to the [Google Cloud Console](https://console.cloud.google.com/), where they'll need to create a new project and set up OAuth consent by providing the necessary information.
2. **Obtain OAuth2 Credentials**: Guide them to create OAuth2 client IDs to obtain the client ID and secret, which will be used in the application.
3. **Install a Suitable Library**: For integrating Google OAuth2, install a library like `vue-google-oauth2` that simplifies the process.
    
    ```bash
    npm install vue-google-oauth2
    
    ```
    
4. **Initialize the Google OAuth Client**: In the Vue.js application (e.g., in `main.js`), initialize the Google OAuth client with the obtained credentials.
    
    ```jsx
    import GAuth from 'vue-google-oauth2'
    
    const gauthOption = {
      clientId: 'YOUR_GOOGLE_CLIENT_ID.apps.googleusercontent.com',
      scope: 'profile email',
      prompt: 'select_account'
    }
    
    createApp(App).use(GAuth, gauthOption).mount('#app')
    
    ```
    

### Implementing Google Sign-In

1. **Add a Google Sign-In Button**: In the user interface, add a button that users can click to sign in with Google. This can be part of your login component.
    
    ```
    <template>
      <v-button @click="googleSignIn">Sign in with Google</v-button>
    </template>
    
    <script>
    export default {
      methods: {
        googleSignIn() {
          this.$gAuth.signIn().then(GoogleUser => {
            // Handle the sign-in response
            // Send token to backend for verification
          }).catch(err => {
            console.error(err);
          });
        }
      }
    };
    </script>
    
    ```
    
2. **Handling the Authentication Response**: After a successful sign-in, you'll receive a Google user object. Extract the ID token and send it to your backend for verification.
    
    ```jsx
    // In the googleSignIn method
    this.$gAuth.signIn().then(GoogleUser => {
      const idToken = GoogleUser.getAuthResponse().id_token;
      // Send the idToken to your backend for verification and user creation/log in
    }).catch(err => {
      console.error(err);
    });
    
    ```
    

### Backend Verification

1. **Verify Token in Backend**: The backend should have a route to handle Google OAuth2 sign-ins, where it verifies the ID token and authenticates or creates the user in your system.

### Tips for Participants

- **Security**: Ensure that the OAuth2 flow is secure and that tokens are handled confidentially.
- **Error Handling**: Implement proper error handling for the OAuth2 process.
- **User Experience**: Focus on making the OAuth2 sign-in process seamless and integrated with the existing authentication system.



## Bonus Step

Feel free to explore more advanced Vue.js and Vuetify features and expand on the exercises provided.

## Feedback

Your feedback is invaluable in improving future workshops. Please share your thoughts and suggestions about this workshop.

Thank you for participating in our Vue.js and Vuetify Workshop!

## Contacts Us

**Malek Gatoufi**: [malek.gatoufi@epitech.eu](mailto:malek.gatoufi@epitech.eu)

**Bastien Rodriguez**: [bastien.rodriguez@epitech.eu](mailto:bastien.rodriguez@epitech.eu)
