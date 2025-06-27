# Node.js CRUD API

A RESTful API built with Node.js, Express, and MongoDB that provides endpoints for managing users and blog posts.

## Features

- **User Management**: Create, read, update, and delete users (MongoDB storage)
- **Blog Management**: Create, read, update, and delete blog posts (MongoDB storage)
- **CORS enabled**: Ready for frontend integration
- **Error handling**: Graceful error responses
- **Environment configuration**: Secure database connection
- **Data validation**: Email uniqueness and required field validation
- **Automatic timestamps**: Created and updated timestamps for all records

## Prerequisites

- Node.js (v14 or higher)
- MongoDB Atlas account (required for both users and blog features)
- npm or yarn

## Installation

1. Clone the repository:
```bash
git clone <your-repo-url>
cd node-crud-api
```

2. Install dependencies:
```bash
npm install
```

3. Set up environment variables:
   - Create a `.env` file in the root directory
   - Add your MongoDB connection string:
```env
MONGODB_URI=your_mongodb_atlas_connection_string
PORT=5000
```

4. Start the server:
```bash
npm start
```

The server will run on `http://localhost:5000`

## API Endpoints

### User Endpoints (MongoDB Storage)

#### Get All Users
- **URL**: `/users`
- **Method**: `GET`
- **Description**: Retrieve all users (sorted by creation date, newest first)
- **Response**: Array of user objects

```javascript
// Example Response
[
  {
    "_id": "65f1234567890abcdef12345",
    "name": "John Doe",
    "email": "john@example.com",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T10:30:00.000Z"
  }
]
```

#### Get Single User
- **URL**: `/users/:id`
- **Method**: `GET`
- **Description**: Retrieve a single user by ID
- **Parameters**: `id` (MongoDB ObjectId)

```javascript
// Example Response
{
  "_id": "65f1234567890abcdef12345",
  "name": "John Doe",
  "email": "john@example.com",
  "createdAt": "2024-01-15T10:30:00.000Z",
  "updatedAt": "2024-01-15T10:30:00.000Z"
}
```

#### Create User
- **URL**: `/users`
- **Method**: `POST`
- **Description**: Create a new user
- **Body**:
```json
{
  "name": "John Doe",
  "email": "john@example.com"
}
```
- **Validation**: Name and email are required, email must be unique

#### Update User
- **URL**: `/users/:id`
- **Method**: `PUT`
- **Description**: Update an existing user
- **Parameters**: `id` (MongoDB ObjectId)
- **Body**:
```json
{
  "name": "John Updated",
  "email": "john.updated@example.com"
}
```
- **Validation**: Email must be unique if changed

#### Delete User
- **URL**: `/users/:id`
- **Method**: `DELETE`
- **Description**: Delete a user by ID
- **Parameters**: `id` (MongoDB ObjectId)
- **Response**: `204 No Content`

### Blog Post Endpoints (MongoDB Storage)

#### Get All Posts
- **URL**: `/api/posts`
- **Method**: `GET`
- **Description**: Retrieve all blog posts (sorted by creation date, newest first)

```javascript
// Example Response
[
  {
    "_id": "65f1234567890abcdef12345",
    "title": "My First Blog Post",
    "content": "This is the content of my first blog post...",
    "author": "John Doe",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T10:30:00.000Z"
  }
]
```

#### Get Single Post
- **URL**: `/api/posts/:id`
- **Method**: `GET`
- **Description**: Retrieve a single blog post by ID
- **Parameters**: `id` (MongoDB ObjectId)

#### Create Post
- **URL**: `/api/posts`
- **Method**: `POST`
- **Description**: Create a new blog post
- **Body**:
```json
{
  "title": "My Blog Post Title",
  "content": "The content of the blog post goes here...",
  "author": "Author Name"
}
```

#### Update Post
- **URL**: `/api/posts/:id`
- **Method**: `PUT`
- **Description**: Update an existing blog post
- **Parameters**: `id` (MongoDB ObjectId)
- **Body**:
```json
{
  "title": "Updated Title",
  "content": "Updated content...",
  "author": "Updated Author"
}
```

#### Delete Post
- **URL**: `/api/posts/:id`
- **Method**: `DELETE`
- **Description**: Delete a blog post by ID
- **Parameters**: `id` (MongoDB ObjectId)

## Frontend Integration Examples

### JavaScript/Fetch API

```javascript
const API_BASE_URL = 'http://localhost:5000';

// Get all users
async function getUsers() {
  try {
    const response = await fetch(`${API_BASE_URL}/users`);
    const users = await response.json();
    return users;
  } catch (error) {
    console.error('Error fetching users:', error);
  }
}

// Create a new user
async function createUser(userData) {
  try {
    const response = await fetch(`${API_BASE_URL}/users`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(userData)
    });
    const newUser = await response.json();
    return newUser;
  } catch (error) {
    console.error('Error creating user:', error);
  }
}

// Get all blog posts
async function getPosts() {
  try {
    const response = await fetch(`${API_BASE_URL}/api/posts`);
    const posts = await response.json();
    return posts;
  } catch (error) {
    console.error('Error fetching posts:', error);
  }
}

// Create a new blog post
async function createPost(postData) {
  try {
    const response = await fetch(`${API_BASE_URL}/api/posts`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(postData)
    });
    const newPost = await response.json();
    return newPost;
  } catch (error) {
    console.error('Error creating post:', error);
  }
}
```

### React Example

```jsx
import React, { useState, useEffect } from 'react';

const API_BASE_URL = 'http://localhost:5000';

function App() {
  const [users, setUsers] = useState([]);
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    fetchUsers();
    fetchPosts();
  }, []);

  const fetchUsers = async () => {
    try {
      const response = await fetch(`${API_BASE_URL}/users`);
      const userData = await response.json();
      setUsers(userData);
    } catch (error) {
      console.error('Error fetching users:', error);
    }
  };

  const fetchPosts = async () => {
    try {
      const response = await fetch(`${API_BASE_URL}/api/posts`);
      const postData = await response.json();
      setPosts(postData);
    } catch (error) {
      console.error('Error fetching posts:', error);
    }
  };

  return (
    <div>
      <h1>Users</h1>
      <ul>
        {users.map(user => (
          <li key={user._id}>{user.name} - {user.email}</li>
        ))}
      </ul>

      <h1>Blog Posts</h1>
      <ul>
        {posts.map(post => (
          <li key={post._id}>
            <h3>{post.title}</h3>
            <p>By: {post.author}</p>
            <p>{post.content}</p>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

### Axios Example

```javascript
import axios from 'axios';

const api = axios.create({
  baseURL: 'http://localhost:5000',
  headers: {
    'Content-Type': 'application/json',
  },
});

// User operations
export const userAPI = {
  getAll: () => api.get('/users'),
  getById: (id) => api.get(`/users/${id}`),
  create: (userData) => api.post('/users', userData),
  update: (id, userData) => api.put(`/users/${id}`, userData),
  delete: (id) => api.delete(`/users/${id}`),
};

// Blog post operations
export const postAPI = {
  getAll: () => api.get('/api/posts'),
  getById: (id) => api.get(`/api/posts/${id}`),
  create: (postData) => api.post('/api/posts', postData),
  update: (id, postData) => api.put(`/api/posts/${id}`, postData),
  delete: (id) => api.delete(`/api/posts/${id}`),
};
```

## Error Handling

The API returns appropriate HTTP status codes and error messages:

- `200` - Success
- `201` - Created
- `204` - No Content (for successful deletions)
- `400` - Bad Request (validation errors)
- `404` - Not Found
- `500` - Internal Server Error
- `503` - Service Unavailable (MongoDB not connected)

### Error Response Format

```json
{
  "error": "Error message description"
}
```

**Common Error Messages:**
- `"Name and email are required"` - Missing required fields
- `"Email already exists"` - Duplicate email address
- `"User not found"` - Invalid user ID or user doesn't exist
- `"Invalid user ID format"` - Malformed MongoDB ObjectId
- `"Database not available. Please check MongoDB connection."` - MongoDB connection issue

For blog posts:

```json
{
  "message": "Error message description"
}
```

## CORS Configuration

The API is configured to accept requests from any origin. For production, you should configure CORS to only allow requests from your frontend domain:

```javascript
app.use(cors({
  origin: 'https://your-frontend-domain.com'
}));
```

## Development

1. Install nodemon for development:
```bash
npm install -g nodemon
```

2. Run in development mode:
```bash
npm run dev
```

## Production Deployment

1. Set up environment variables on your hosting platform
2. Ensure MongoDB Atlas is accessible
3. Update CORS settings for your production domain
4. Use `npm start` to run the production server

## Notes

- **All data is now stored in MongoDB** and persists between server restarts
- **Email validation**: User emails must be unique across the system
- **MongoDB connection required**: Both user and blog operations require MongoDB to be connected
- **ObjectId format**: All user and blog post IDs use MongoDB ObjectId format
- **Automatic timestamps**: All records include `createdAt` and `updatedAt` fields
- **Data validation**: Server-side validation for required fields and data integrity
- The API includes middleware to check MongoDB connection status for all routes

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is licensed under the MIT License.
