
# CHAT WITH PDF

Three phases:
1. Local setup and frontend.
2. AI logic and backend.
3. Vercel deployment.




## 1. my-chat-app/
├── frontend/               # 📍 YOUR UI
│   ├── index.html
│   ├── style.css
│   └── script.js
│
├── api/                    # ⚙️ YOUR PYTHON BACKEND
│   ├── main.py             # Your FastAPI or Flask code
│   └── requirements.txt    # Lists your Python libraries (e.g., fastapi, google-generativeai)
│
└── vercel.json             # 🗺️ THE ROUTING MAPLocal setup and front-end

- [ ] Need to figure out the feature that needed to be added to the app.
	- [x] Need to add /api/chat feature.
	- [/] currently fine for the basic use need to add more features after the basic usage is over. 
- [x] Build the layout for the above features.
- [ ] Need to add state variables and make app frontend ready.

Run the app:
1. run the server first python3 -m http.server 8000

---
## 2. AI logic and back-end

HOW TO RUN APP:
1. need to make a normal ai chatbot with the project first.
	*source .venv/bin/activate*
	*uvicorn api.main:app --reload & python3 -m http.server 3000 --directory frontend*

-TODO:
1. need to add streaming response.
2. need to add previous context of the message.
3. need to correct the font end scrolling.
	
- [x] use google studio for getting the API for free Gemini API-
- [ ] integrate the AI SDK *(need to look into it).*

---
## 3. Vercel deployment
- [ ] push to github 
- [ ] import to vercel
- [ ] inject secrets securely
- [ ] configure server-less Timeout.
- [ ] DEPLOY
 


[[3.3 Learning from example project.]]