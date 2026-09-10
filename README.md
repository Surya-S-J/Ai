# Ai
Personal ai assistant 
import os
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from openai import OpenAI

app = FastAPI(title="Surya AI Backend", version="1.0.0")

class ChatRequest(BaseModel):
    message: str

@app.get("/health")
def health():
    return {"status": "ok", "assistant": "Surya AI"}

@app.post("/chat")
def chat(req: ChatRequest):
    key = os.getenv("OPENAI_API_KEY")
    if not key:
        raise HTTPException(503, "OPENAI_API_KEY is not configured on the backend")
    client = OpenAI(api_key=key)
    response = client.responses.create(model=os.getenv("OPENAI_MODEL", "gpt-5.6"), input=req.message)
    return {"reply": response.output_text}
