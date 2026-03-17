# Command Log – LLM Deployment Tuning

This document captures the iterative process used to achieve stable CUDA-accelerated local inference with `llama.cpp`.

---

## Final Stable Configuration

```bash
./bin/llama-cli \
  -m ../models/mistral-7b-instruct-v0.2.Q4_K_M.gguf \
  -ngl 40 \
  -c 4096 \
  -i --interactive-first
```

---

## 1. Model Loading & Path Resolution

```bash
~/llama.cpp/build/bin/llama-cli \
  -m ./mistral-7b-instruct-v0.2.Q4_K_M.gguf

/home/codebabe/llama.cpp/build/bin/llama-cli \
  -m /home/codebabe/models/mistral-7b-instruct-v0.2.Q4_K_M.gguf

./bin/llama-cli \
  -m ../models/mistral-7b-instruct-v0.2.Q4_K_M.gguf
```

Notes:
- Tested relative vs absolute paths  
- Standardized on `../models/` structure  

---

## 2. GPU Acceleration & Layer Offloading

```bash
./bin/llama-cli \
  -m ../models/mistral-7b-instruct-v0.2.Q4_K_M.gguf \
  --gpu-layers 40

./bin/llama-cli \
  -m ../models/mistral-7b-instruct-v0.2.Q4_K_M.gguf \
  -ngl 40
```

Notes:
- Transitioned from `--gpu-layers` → `-ngl`  
- Verified stable GPU offload at 40 layers  

---

## 3. Sampling & Response Tuning

```bash
--temp 0.9 --top-p 0.95 --repeat-penalty 1.1
--temp 0.85 --top-p 0.92 --repeat-penalty 1.12
```

Notes:
- Tuned for balance between creativity and coherence  
- Adjusted to stabilize conversational responses  

---

## 4. Prompt & Persona Experiments (Virion)

```bash
-p "<s>[INST] You are Virion... [/INST]"

-p "You are VIRION, CodeBabe’s glitchcore co-architect..."

--sys "You are Virion. Reply immediately and keep replies short."
```

Notes:
- Tested multiple instruction formats (`[INST]`, inline prompts, system prompts)  
- Developed persistent persona behavior  

---

## 5. Interactive Mode & Chat Behavior

```bash
-i
--interactive-first
--color
--reverse-prompt "User:"
--in-prefix "User: "
--in-suffix "\nVirion: "
```

Notes:
- Established conversational loop  
- Tested role formatting and response triggers  

---

## 6. Session & Context Handling

```bash
--session virion.session --session-out
-c 4096
-n -1
```

Notes:
- Enabled session persistence  
- Increased context window for extended interaction  

---

## 7. Recovery & Model Re-download

```bash
find .. -maxdepth 4 -type f -name "*.gguf" -o -name "*.bin"

wget https://huggingface.co/TheBloke/Mistral-7B-Instruct-v0.2-GGUF/resolve/main/mistral-7b-instruct-v0.2.Q4_K_M.gguf
```

Notes:
- Located model files after path issues  
- Re-downloaded model for clean environment recovery  

---

## 8. Alternative Interface Testing

```bash
./bin/llama-chat \
  -m ../models/mistral-7b-instruct-v0.2.Q4_K_M.gguf \
  -ngl 40 \
  -c 4096
```

Notes:
- Compared `llama-cli` vs `llama-chat`  
- Final preference: `llama-cli` for greater control
