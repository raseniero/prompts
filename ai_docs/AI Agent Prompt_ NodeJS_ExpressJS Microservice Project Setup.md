# **AI Agent Prompt: NodeJS/ExpressJS Microservice Project Setup**

**Objective:**

Generate a new Node.js/Express.js microservice project that adheres to the provided template. This template is designed for building scalable, maintainable, and robust microservices. It emphasizes a clear separation of concerns, modularity, and best practices for both development and production environments.

**Project Structure:**

Create the following folder and file structure. This component-driven structure is a critical part of the template. Do not deviate.

/  
|-- .dockerignore  
|-- .env  
|-- .env.example  
|-- .gitignore  
|-- docker-compose.yml  
|-- Dockerfile  
|-- package.json  
|-- README.md  
|-- src/  
|   |-- components/  
|   |   |-- users/  
|   |   |   |-- index.ts  
|   |   |   |-- users.controller.ts  
|   |   |   |-- users.model.ts  
|   |   |   |-- users.routes.ts  
|   |   |   |-- users.service.ts  
|   |   |   |-- users.test.ts  
|   |   |   |-- users.validation.ts  
|   |-- common/  
|   |   |-- middlewares/  
|   |   |   |-- auth.middleware.js  
|   |   |   |-- error.middleware.js  
|   |   |-- utils/  
|   |   |   |-- AppError.js  
|   |   |   |-- catchAsync.js  
|   |-- config/  
|   |   |-- config.js  
|   |   |-- logger.js  
|   |-- app.js  
|   |-- server.js  
|-- tests/  
|   |-- fixtures/  
|   |   |-- user.fixture.js  
|   |-- setup.js

**File Content and Instructions:**

**1\. Root Directory:**

* **.gitignore**: Standard Node.js gitignore file. Include node\_modules, .env, coverage, build, and log files.  
* **.env.example**: Example environment variables. Include PORT, MONGODB\_URL, JWT\_SECRET, NODE\_ENV.  
* **.env**: To be created by the user, based on .env.example.  
* **package.json**:  
  * Include the following scripts:  
    * start: node dist/server.js  
    * dev: nodemon src/server.ts  
    * test: jest \--verbose  
  * Include the following dependencies: express, mongoose, dotenv, joi, winston, cors, helmet, http-status-codes, jsonwebtoken, bcryptjs.  
  * Include the following dev dependencies: jest, supertest, nodemon, eslint, prettier, typescript, @types/node, @types/express.  
* **Dockerfile**: A multi-stage Dockerfile for building a production-ready image.  
* **docker-compose.yml**: For local development, set up the Node.js service and a MongoDB service.  
* **README.md**: A basic README with project setup instructions.

**2\. src Directory:**

* **app.js**:  
  * Require express, helmet, cors.  
  * Use middleware: express.json(), express.urlencoded({ extended: true }), helmet(), cors().  
  * Mount the routers from each component in the src/components directory.  
  * Include a 404 handler for unknown API requests.  
  * Include the global error handler middleware from src/common/middlewares.  
* **server.js**:  
  * The main entry point of the application.  
  * Import the app from app.js.  
  * Import the logger and config.  
  * Start the Express server on the configured port.  
  * Handle unhandledRejection and uncaughtException events.

**3\. src/components/ (Business Logic)**

* Create an example component named users. This component will serve as the template for all future components.  
* **src/components/users/**:  
  * **users.routes.ts**: Defines API endpoints for the user component (e.g., GET /, POST /).  
  * **users.controller.ts**: Handles request/response logic, calling the service layer. Contains no business logic.  
  * **users.service.ts**: Contains the core business logic for the user component.  
  * **users.model.ts**: Defines the data schema (you can use a placeholder interface for this example).  
  * **users.validation.ts**: Defines data validation schemas (e.g., using Joi or class-validator, but a placeholder is fine).  
  * **users.test.ts**: Contains example unit/integration tests for the user component using Jest and Supertest.  
  * **index.ts**: Exports the component's router to be mounted in app.ts.

**4\. src/common/ (Shared Logic)**

* **middlewares/error.middleware.js**:  
  * A centralized error-handling middleware.  
  * Handle different types of errors (e.g., AppError, Mongoose errors, Joi validation errors).  
  * Send a standardized error response.  
* **utils/AppError.js**:  
  * A custom Error class for operational errors.  
  * Should extend the built-in Error class and include a statusCode and isOperational property.  
* **utils/catchAsync.js**:  
  * A higher-order function to wrap async controller functions and catch any errors, passing them to the global error handler.

**5\. src/config Directory:**

* **config.js**:  
  * Load environment variables from the .env file using dotenv.  
  * Export an object with all configuration variables.  
* **logger.js**:  
  * Configure winston for logging.  
  * Create different transports for console and file logging based on the environment.

**6\. tests Directory:**

* This directory is for global test configuration and assets. Component-specific tests should be co-located within their respective component directories.  
* **setup.js**: Jest setup file for tasks like connecting to a test database before tests run.  
* **fixtures/user.fixture.js**: Sample user data for tests.

**Final Instructions:**

* Ensure all code is well-commented, especially the purpose of each file and function.  
* Adhere to a consistent coding style (e.g., using a linter like ESLint and a formatter like Prettier).  
* The generated project should be ready to run with npm install and npm run dev.

This prompt will guide the AI agent to generate a complete and well-structured Express.js/Node.js microservice project that is easy to understand, maintain, and scale.