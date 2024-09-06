This project is a Node.js API for user authentication using JWT (JSON Web Tokens). It utilizes `Express` for the server framework, `Mongoose` for interacting with MongoDB, and `bcryptjs` for password hashing.

### Project Structure and Explanation:

1. **Dependencies:**
   - `bcryptjs`: Used to hash passwords securely before storing them in the database.
   - `cors`: Middleware to enable Cross-Origin Resource Sharing, which is essential for front-end apps to communicate with the API from different origins.
   - `dotenv`: Loads environment variables from a `.env` file (e.g., MongoDB URI, JWT secret).
   - `express`: Web framework for building APIs and handling HTTP requests.
   - `jsonwebtoken`: For generating and verifying JWT tokens for authentication.
   - `mongoose`: Object Data Modeling (ODM) library for MongoDB, allowing interaction with the MongoDB database.

2. **Authentication Routes (`authRoutes`)**:
   - **Register Route (`POST /api/auth/register`)**:
     - Receives username and password from the client.
     - Checks if a user with the same username already exists. If yes, it responds with an error.
     - If the user doesn't exist, it hashes the password using `bcryptjs` and creates a new user in the database.
     - Returns a success message once the user is successfully registered.

   - **Login Route (`POST /api/auth/login`)**:
     - Receives the username and password from the client.
     - Checks if the user exists in the database.
     - If the user exists, it compares the provided password with the stored hashed password using `bcrypt.compare`.
     - If the password matches, it generates a JWT token, signs it with the secret key from the environment (`process.env.JWT_SECRET`), and sends the token back to the client. The token expires after 1 hour.

   - **Get All Users Route (`GET /api/auth/users`)**:
     - Retrieves all users from the database.
     - It excludes the password from the response using `.select('-password')` to ensure sensitive data is not exposed.

   - **Add New User Route (`POST /api/auth/users`)**:
     - Similar to the registration route, but allows adding users via this route as well.
     - Checks if the user already exists, and if not, hashes the password and adds the user to the database.

   - **Update User Route (`PATCH /api/auth/users/:id`)**:
     - Updates user information based on the ID parameter from the URL.
     - If a password is provided, it re-hashes the new password before saving.
     - Updates the username if provided and saves the updated user to the database.

   - **Delete User Route (`DELETE /api/auth/users/:id`)**:
     - Deletes a user based on the provided ID.
     - If the user exists, it removes the user from the database and responds with a success message.

3. **Server Setup (`index.js`)**:
   - Sets up an Express app.
   - Uses `express.json()` to parse incoming JSON requests.
   - Applies the `cors` middleware to enable cross-origin requests.
   - Imports and uses the authentication routes (`authRoutes`).
   - Connects to the MongoDB database using `mongoose.connect()`, with the MongoDB URI fetched from the `.env` file.
   - Starts the server on the specified port (either from the environment or default `5000`).

4. **Environment Variables (`.env` file)**:
   - `PORT`: Defines the port on which the server will run.
   - `MONGO_URI`: MongoDB connection URI for storing and retrieving user data.
   - `JWT_SECRET`: Secret key used to sign and verify JWT tokens.

### Summary:
This project allows user registration, login, and basic CRUD (Create, Read, Update, Delete) operations on user data. It handles password hashing with `bcryptjs` for security, and JWT is used for session management and user authentication. The API is designed to communicate securely with a front-end client, ensuring that sensitive data like passwords are hashed and JWT tokens are used for secure access.
