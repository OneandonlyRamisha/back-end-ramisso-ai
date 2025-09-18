# Overview

Template for AI-powered chatbots for ecommerce stores.
Single codebase can be customized per client by updating variables in index.js and embed.min.js.

Key Features:
- Connects to OpenAI API via WebSocket.
- Handles daily message limits per user.
- Pre-trained prompt for “Ramisso AI” agency persona.
- Responsive chat UI with typewriter effect.
- Easily customizable branding and messages.

# Backend (index.js)
Dependencies:
- express
- openai
- dotenv
- ws

### Config
- config.env stores:
- PORT – server port
- OPENAI_API_KEY – OpenAI key

### Variables
- trainedPropt – The main AI persona prompt. Change here to customize agency tone, audience, or services.
- MAX_MESSAGES – Max messages per user per day (default 20).
- RESET_INTERVAL – Time window for message limit (default 24h).
- userLimits – Tracks message counts per IP.
- ALLOWED_ORIGIN – The only website allowed to connect (e.g., https://clientwebsite.com). ✅ Update per client
- CLIENT_TOKEN – Secret token required to connect via WebSocket. Must match frontend WS_TOKEN. ✅ Update per client


### Functions / Flow
Express server serves optional static files (public folder).
WebSocketServer listens on /chat.
On user connection:
- Initialize/reset userLimits.
- Check daily message limit.
- Call OpenAI API with trainedPropt and user message.
- Send AI response back to client.
- Error handling: fallback message if AI fails.



# Frontend (public/embed.min.js)
- Structure
- Trigger button – #trigger-chat-btn
- Chat container – #my-ai-chatbot
- Header – #chat-header
- Messages container – #chat-messages
- Input container – #chat-input-container
- Input field – #chat-input
- Send button – #chat-send-btn


### Key Variables
- ws – WebSocket connection (update URL for deployment)
- typeWriter(text, container) – Animates bot text.
- sendMessage() – Handles sending user messages, displaying user msg, loading dots, and receiving bot response.
- Customizable Elements
- Branding: brand-name, brand-subname, logo src
- Colors: CSS variables at top (--bg-chat, --bot-msg-bg, etc.)
- Welcome message: welcomeMsg.innerText
- WebSocket URL: change from local to production URL
- WS_TOKEN – Secret token sent to backend via WebSocket URL. Must match backend CLIENT_TOKEN. ✅ Update per client
- WS_URL – Full WebSocket URL including token. Update for deployment and token. ✅ Update per client



### Customization Checklist
- Update trainedPropt in index.js with client-specific info.
- Change branding in embed.min.js:
- Logo, brand name, subname
- Colors if desired
- Welcome message
- Update ws URL for production environment.
- Adjust MAX_MESSAGES if needed.
- Deploy backend with node index.js or preferred hosting (Render, Vercel, etc.)
- Embed embed.min.js in client site.
- Update ALLOWED_ORIGIN in index.js with client website URL.
- Generate CLIENT_TOKEN in config.env and use the same WS_TOKEN in embed.min.js.
- Update WS_URL in embed.min.js to include the token.


### Quick Commands:
#Start backend
node index.js
#Frontend
Include <script src="embed.min.js"></script> in site


### Notes
- Keep OpenAI key private.
- trainedPropt is the most important file to edit for new clients.
- Only minor frontend tweaks usually needed per client.
- Daily message limits prevent abuse; adjust if required.



| **File / Section**                  | **Variable / Function**                                                | **Purpose / Description**                               | **Editable for New Client**                                  |
| ----------------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| `config.env`                        | `PORT`                                                                 | Server port                                             | ✅ Optional                                                   |
|                                     | `OPENAI_API_KEY`                                                       | OpenAI API key                                          | ❌ Keep secret                                                |
|                                     | `CLIENT_TOKEN`                                                         | Secret token required for WebSocket authentication      | ✅ Change per client                                          |
| `index.js`                          | `trainedPropt`                                                         | Main AI persona & instructions for chatbot              | ✅ Change agency/client tone, target audience, promises, etc. |
|                                     | `MAX_MESSAGES`                                                         | Max messages per user per day                           | ✅ Adjust if needed                                           |
|                                     | `RESET_INTERVAL`                                                       | Time window for message limit (default 24h)             | ✅ Optional                                                   |
|                                     | `userLimits`                                                           | Tracks user message counts per IP                       | ❌ Keep as-is                                                 |
|                                     | WebSocketServer `wss`                                                  | Handles all chat connections                            | ❌ Keep as-is                                                 |
|                                     | `ws.on("message")`                                                     | Receives messages, calls OpenAI, sends response         | ❌ Logic should not change                                    |
|                                     | `ALLOWED_ORIGIN`                                                       | Only allowed website for connections                    | ✅ Change per client                                          |
| `public/embed.min.js`               | `#trigger-chat-btn`                                                    | Button to open chat                                     | ✅ Optional style tweak                                       |
|                                     | `#my-ai-chatbot`                                                       | Main chat container                                     | ✅ Optional style tweak                                       |
|                                     | `#chat-header`                                                         | Chat header with logo & brand                           | ✅ Update logo, brand name/subname                            |
|                                     | `#chat-messages`                                                       | Container for all chat messages                         | ❌ Keep as-is                                                 |
|                                     | `#chat-input`                                                          | User input field                                        | ❌ Keep as-is                                                 |
|                                     | `#chat-send-btn`                                                       | Send button                                             | ✅ Optional style tweak                                       |
|                                     | `ws`                                                                   | WebSocket connection to backend (include token)         | ✅ Update for production / new backend URL and token          |
|                                     | `WS_TOKEN`                                                             | Secret token included in WebSocket URL                  | ✅ Update per client                                          |
|                                     | `WS_URL`                                                               | Full WebSocket URL including token                      | ✅ Update per client                                          |
|                                     | `typeWriter(text, container)`                                          | Typewriter animation for bot text                       | ❌ Keep as-is                                                 |
|                                     | `sendMessage()`                                                        | Sends user input, displays loader, handles bot response | ❌ Keep as-is                                                 |
| CSS variables (top of embed.min.js) | `--bg-chat`, `--bot-msg-bg`, `--user-msg-bg`, `--accent-gradient` etc. | Colors, gradients, fonts                                | ✅ Optional styling per client                                |
| Welcome message                     | `welcomeMsg.innerText`                                                 | First bot message when chat opens                       | ✅ Update per client                                          |
                                         |

