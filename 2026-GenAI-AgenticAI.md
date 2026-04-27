# Section 8: GenAI & Innovation (Unified Format)

---

## Q0: How are you using GenAI

### Core Answer
- Use ML to identify high-risk patients  
- Use GenAI to explain risk in natural language  
- Improves usability and decision-making  

### If Probed
- Combine ML + GenAI for explainability  
- Focus on workflow integration  

---

## Q1: How do you design GenAI solution

### Core Answer
- Input → process → output  
- Generate insights from data  

### If Probed
- Use layered architecture  
- Combine RAG + direct query  

---

## Q2: End-to-End GenAI Architecture

**Memory Trick**  
Access → Orchestrate → Retrieve → LLM → Guard → Observe  

### Core Answer
- Access layer (API, auth)  
- Orchestration (prompt, workflow)  
- Retrieval (data fetch)  
- LLM (generation)  
- Guardrails (security, validation)  
- Observability (monitoring)  

### If Probed
- RAG vs direct query decision  
- Model abstraction  
- Cost optimization  
- Multi-tenant governance  

---

## Q3: Build vs Buy

### Core Answer
- Prefer using existing models  
- Build integration and logic  

### If Probed
- Buy models  
- Build orchestration + domain layer  
- Decide based on cost and speed  

---

## Q4: Prompt Engineering

### Core Answer
- Use structured prompts  
- Provide context and expected output  

### If Probed
- Use framework: role, task, context, constraints  
- Maintain templates and versioning  

---

## Q5: RAG vs Direct Query

### Core Answer
- RAG for unstructured  
- Direct query for structured  

### If Probed
- Avoid RAG for low latency or deterministic systems  
- Use hybrid approach  

---

## Q6: Chunking & Embeddings

### Core Answer
- Break data into chunks  
- Convert to embeddings  
- Retrieve relevant data  

### If Probed
- Semantic chunking with overlap  
- Vector similarity search  

---

## Q7: Hallucination handling

### Core Answer
- Use validation and control  

### If Probed
- Grounding via retrieval  
- Prompt constraints  
- Output validation  
- Human-in-loop  

---

## Q8: Security

### Core Answer
- Secure data access  
- Role-based access  

### If Probed
- Data isolation  
- No direct LLM exposure  
- Audit logging  

---

## Q9: Cost vs Performance

### Core Answer
- Balance cost and performance  

### If Probed
- Optimize tokens, caching  
- Use smaller models when possible  

---

## Q10: Scaling GenAI

### Core Answer
- Scale system components  

### If Probed
- Rate limiting  
- Async processing  
- Model optimization  
- Cost control  

---

## Q11: Measuring success

### Core Answer
- Efficiency improvement  
- Better decisions  

### If Probed
- Adoption  
- cost per request  
- business impact  

---