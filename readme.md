\# Video Call Backend Server



A Node.js backend server for the video calling application, built with Express and Socket.io. This server handles user authentication, real-time signaling for WebRTC connections, and manages user presence.



\## 🚀 Features



\- \*\*Real-time Signaling\*\* - WebRTC signaling server using Socket.io

\- \*\*User Management\*\* - In-memory user storage and presence tracking

\- \*\*Call Management\*\* - Handle call initiation, acceptance, rejection, and termination

\- \*\*Authentication API\*\* - Simple REST API for user login

\- \*\*ICE Candidate Exchange\*\* - Facilitate peer-to-peer connection establishment

\- \*\*Connection Health\*\* - Ping/pong for connection monitoring



\## 📋 Prerequisites



\- \*\*Node.js\*\* (v16 or higher)

\- \*\*npm\*\* or \*\*yarn\*\*



\## 🛠️ Installation



\### 1. Navigate to Server Directory

```bash

cd server

```



\### 2. Install Dependencies

```bash

npm install

```



\### 3. Start the Server

```bash

\# Production

npm start



\# Development (with auto-restart)

npm run dev

```



The server will start on `http://localhost:3001`



\## 📦 Dependencies



\### Production Dependencies

```json

{

&nbsp; "express": "^4.18.2",      // Web framework

&nbsp; "socket.io": "^4.7.2",     // WebSocket server

&nbsp; "cors": "^2.8.5",          // Cross-origin requests

&nbsp; "uuid": "^9.0.0"           // Unique ID generation

}

```



\### Development Dependencies

```json

{

&nbsp; "nodemon": "^3.0.1"        // Auto-restart during development

}

```



\## 🏗️ Server Architecture



```

server/

├── index.js              # Main server file

├── package.json           # Dependencies and scripts

├── package-lock.json      # Dependency lock file

└── README.md             # This file

```



\## 🔌 API Endpoints



\### REST API



\#### POST `/api/login`

Authenticate a user and return user data.



\*\*Request:\*\*

```json

{

&nbsp; "username": "john\_doe",

&nbsp; "password": "any\_password"

}

```



\*\*Response:\*\*

```json

{

&nbsp; "user": {

&nbsp;   "id": "uuid-string",

&nbsp;   "username": "john\_doe",

&nbsp;   "isOnline": false

&nbsp; },

&nbsp; "token": "uuid-string"

}

```



\*\*Error Response:\*\*

```json

{

&nbsp; "error": "Username and password required"

}

```



\#### GET `/api/users`

Get all users (online and offline).



\*\*Response:\*\*

```json

\[

&nbsp; {

&nbsp;   "id": "uuid-string",

&nbsp;   "username": "john\_doe",

&nbsp;   "isOnline": true

&nbsp; }

]

```



\#### GET `/api/online-users`

Get only online users.



\*\*Response:\*\*

```json

\[

&nbsp; {

&nbsp;   "id": "uuid-string",

&nbsp;   "username": "john\_doe",

&nbsp;   "isOnline": true,

&nbsp;   "socketId": "socket-id"

&nbsp; }

]

```



\## 🔄 Socket.io Events



\### Client → Server Events



\#### `join`

User joins the server and goes online.

```javascript

socket.emit('join', {

&nbsp; id: 'user-uuid',

&nbsp; username: 'john\_doe'

});

```



\#### `call\_user`

Initiate a call to another user.

```javascript

socket.emit('call\_user', {

&nbsp; to: 'target-user-id',

&nbsp; from: { id: 'caller-id', username: 'caller' },

&nbsp; callType: 'video', // or 'audio'

&nbsp; offer: rtcSessionDescription

});

```



\#### `accept\_call`

Accept an incoming call.

```javascript

socket.emit('accept\_call', {

&nbsp; callId: 'call-uuid',

&nbsp; answer: rtcSessionDescription

});

```



\#### `reject\_call`

Reject an incoming call.

```javascript

socket.emit('reject\_call', {

&nbsp; callId: 'call-uuid'

});

```



\#### `end\_call`

End an active call.

```javascript

socket.emit('end\_call', {

&nbsp; callId: 'call-uuid'

});

```



\#### `ice\_candidate`

Exchange ICE candidates for WebRTC connection.

```javascript

socket.emit('ice\_candidate', {

&nbsp; to: 'target-user-id',

&nbsp; candidate: rtcIceCandidate

});

```



\#### `ping`

Health check ping.

```javascript

socket.emit('ping');

```



\### Server → Client Events



\#### `join\_success`

Confirmation of successful join.

```javascript

{

&nbsp; message: 'Successfully connected to the server',

&nbsp; onlineUsers: \[/\* array of online users \*/]

}

```



\#### `users\_updated`

Broadcast when user list changes.

```javascript

\[/\* array of currently online users \*/]

```



\#### `incoming\_call`

Notify user of incoming call.

```javascript

{

&nbsp; callId: 'call-uuid',

&nbsp; from: { id: 'caller-id', username: 'caller' },

&nbsp; callType: 'video',

&nbsp; offer: rtcSessionDescription

}

```



\#### `call\_accepted`

Notify caller that call was accepted.

```javascript

{

&nbsp; callId: 'call-uuid',

&nbsp; answer: rtcSessionDescription

}

```



\#### `call\_rejected`

Notify caller that call was rejected.

```javascript

{

&nbsp; callId: 'call-uuid'

}

```



\#### `call\_ended`

Notify that call has ended.

```javascript

{

&nbsp; callId: 'call-uuid'

}

```



\#### `call\_status`

Update on call status.

```javascript

{

&nbsp; status: 'ringing', // or 'user\_offline'

&nbsp; callId: 'call-uuid'

}

```



\#### `ice\_candidate`

Forward ICE candidate to peer.

```javascript

{

&nbsp; from: 'sender-user-id',

&nbsp; candidate: rtcIceCandidate

}

```



\#### `pong`

Response to ping.

```javascript

// No data, just event

```



\## 💾 Data Storage



\### In-Memory Storage

The server uses in-memory storage for demo purposes:



```javascript

const users = new Map();        // All users (persistent)

const activeUsers = new Map();  // Currently online users

const calls = new Map();        // Active calls

```



\### User Object Structure

```javascript

{

&nbsp; id: 'uuid-string',

&nbsp; username: 'john\_doe',

&nbsp; isOnline: true,

&nbsp; socketId: 'socket-id',

&nbsp; lastSeen: '2023-12-01T10:30:00.000Z'

}

```



\### Call Object Structure

```javascript

{

&nbsp; id: 'call-uuid',

&nbsp; from: { id: 'caller-id', username: 'caller' },

&nbsp; to: { id: 'callee-id', username: 'callee' },

&nbsp; callType: 'video',

&nbsp; status: 'ringing',

&nbsp; startTime: 1701423000000

}

```



\## 🔧 Configuration



\### Environment Variables

```bash

PORT=3001                    # Server port (default: 3001)

NODE\_ENV=development         # Environment mode

```



\### CORS Configuration

```javascript

const io = socketIo(server, {

&nbsp; cors: {

&nbsp;   origin: "http://localhost:5173",  // Frontend URL

&nbsp;   methods: \["GET", "POST"]

&nbsp; }

});

```



\### Express Middleware

```javascript

app.use(cors());                    // Enable CORS

app.use(express.json());            // Parse JSON bodies

```



\## 🔍 Logging \& Debugging



\### Console Logs

The server provides detailed logging:

\- 🔌 Connection events

\- 👥 User join/leave events

\- 📞 Call state changes

\- 🧊 ICE candidate exchanges

\- ❌ Error conditions



\### Debug Mode

Enable detailed logging:

```bash

DEBUG=socket.io\* npm start

```



\## 🚀 Deployment



\### Local Development

```bash

npm run dev    # Uses nodemon for auto-restart

```



\### Production Deployment



\#### 1. \*\*Heroku\*\*

```bash

\# Create Procfile

echo "web: node index.js" > Procfile



\# Deploy

git add .

git commit -m "Deploy to Heroku"

git push heroku main

```



\#### 2. \*\*Railway\*\*

```bash

\# railway.json

{

&nbsp; "build": {

&nbsp;   "builder": "NIXPACKS"

&nbsp; },

&nbsp; "deploy": {

&nbsp;   "startCommand": "node index.js"

&nbsp; }

}

```



\#### 3. \*\*DigitalOcean App Platform\*\*

```yaml

\# .do/app.yaml

name: video-call-server

services:

\- name: api

&nbsp; source\_dir: /server

&nbsp; github:

&nbsp;   repo: your-repo

&nbsp;   branch: main

&nbsp; run\_command: node index.js

&nbsp; environment\_slug: node-js

&nbsp; instance\_count: 1

&nbsp; instance\_size\_slug: basic-xxs

```



\### Production Considerations



\#### 1. \*\*Environment Variables\*\*

```bash

PORT=80

NODE\_ENV=production

CORS\_ORIGIN=https://your-frontend-domain.com

```



\#### 2. \*\*Process Management\*\*

```bash

\# Using PM2

npm install -g pm2

pm2 start index.js --name "video-call-server"

pm2 startup

pm2 save

```



\#### 3. \*\*Reverse Proxy (Nginx)\*\*

```nginx

server {

&nbsp;   listen 80;

&nbsp;   server\_name your-domain.com;

&nbsp;   

&nbsp;   location / {

&nbsp;       proxy\_pass http://localhost:3001;

&nbsp;       proxy\_http\_version 1.1;

&nbsp;       proxy\_set\_header Upgrade $http\_upgrade;

&nbsp;       proxy\_set\_header Connection 'upgrade';

&nbsp;       proxy\_set\_header Host $host;

&nbsp;       proxy\_cache\_bypass $http\_upgrade;

&nbsp;   }

}

```



\## 🛡️ Security Considerations



\### Current Limitations (Demo Only)

\- \*\*No real authentication\*\* - accepts any username/password

\- \*\*In-memory storage\*\* - data lost on restart

\- \*\*No rate limiting\*\* - vulnerable to spam

\- \*\*No input validation\*\* - accepts any data

\- \*\*No encryption\*\* - messages sent in plain text



\### Production Security Measures

```javascript

// Rate limiting

const rateLimit = require('express-rate-limit');

app.use(rateLimit({

&nbsp; windowMs: 15 \* 60 \* 1000, // 15 minutes

&nbsp; max: 100 // limit each IP to 100 requests per windowMs

}));



// Input validation

const { body, validationResult } = require('express-validator');

app.post('/api/login', \[

&nbsp; body('username').isLength({ min: 3 }).trim().escape(),

&nbsp; body('password').isLength({ min: 6 })

], (req, res) => {

&nbsp; const errors = validationResult(req);

&nbsp; if (!errors.isEmpty()) {

&nbsp;   return res.status(400).json({ errors: errors.array() });

&nbsp; }

&nbsp; // ... rest of login logic

});



// Helmet for security headers

const helmet = require('helmet');

app.use(helmet());

```



\## 📊 Performance Monitoring



\### Health Check Endpoint

```javascript

app.get('/health', (req, res) => {

&nbsp; res.json({

&nbsp;   status: 'OK',

&nbsp;   timestamp: new Date().toISOString(),

&nbsp;   uptime: process.uptime(),

&nbsp;   memory: process.memoryUsage(),

&nbsp;   activeUsers: activeUsers.size,

&nbsp;   activeCalls: calls.size

&nbsp; });

});

```



\### Metrics to Monitor

\- \*\*Active connections\*\* - Number of connected users

\- \*\*Memory usage\*\* - Server memory consumption

\- \*\*Call success rate\*\* - Percentage of successful calls

\- \*\*Response time\*\* - API endpoint response times



\## 🐛 Troubleshooting



\### Common Issues



\#### 1. \*\*Port Already in Use\*\*

```bash

\# Find process using port 3001

lsof -i :3001



\# Kill the process

kill -9 <PID>

```



\#### 2. \*\*CORS Errors\*\*

\- Update CORS origin in server configuration

\- Ensure frontend URL matches CORS settings



\#### 3. \*\*Socket Connection Failed\*\*

\- Check if server is running

\- Verify firewall settings

\- Ensure WebSocket support is enabled



\#### 4. \*\*Memory Leaks\*\*

\- Monitor memory usage with `process.memoryUsage()`

\- Implement proper cleanup for disconnected users

\- Clear inactive calls periodically



\### Debug Commands

```bash

\# Check server status

curl http://localhost:3001/health



\# Test login endpoint

curl -X POST http://localhost:3001/api/login \\

&nbsp; -H "Content-Type: application/json" \\

&nbsp; -d '{"username":"test","password":"test"}'



\# Monitor logs

tail -f server.log

```



\## 🔄 API Testing



\### Using curl

```bash

\# Test login

curl -X POST http://localhost:3001/api/login \\

&nbsp; -H "Content-Type: application/json" \\

&nbsp; -d '{"username":"testuser","password":"testpass"}'



\# Get users

curl http://localhost:3001/api/users



\# Get online users

curl http://localhost:3001/api/online-users

```



\### Using Postman

Import the following collection:

```json

{

&nbsp; "info": { "name": "Video Call API" },

&nbsp; "item": \[

&nbsp;   {

&nbsp;     "name": "Login",

&nbsp;     "request": {

&nbsp;       "method": "POST",

&nbsp;       "url": "http://localhost:3001/api/login",

&nbsp;       "body": {

&nbsp;         "mode": "raw",

&nbsp;         "raw": "{\\"username\\":\\"test\\",\\"password\\":\\"test\\"}"

&nbsp;       }

&nbsp;     }

&nbsp;   }

&nbsp; ]

}

```



\## 📚 Additional Resources



\- \[Socket.io Documentation](https://socket.io/docs/)

\- \[Express.js Guide](https://expressjs.com/en/guide/)

\- \[WebRTC Signaling](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC\_API/Signaling\_and\_video\_calling)

\- \[Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)



---



\*\*Server ready for video calling! 🚀\*\*

