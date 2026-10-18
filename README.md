# AI Study & Career Guidance Assistant (RAG + AirLLM)

A source-grounded assistant: it answers only from documents in `data/docs/` and cites them.

## Architecture
Documents -> loaders -> chunking (220 words, 40 overlap) -> MiniLM embeddings -> FAISS index
Question -> embed -> top-k retrieval (+ score threshold) -> grounded prompt -> AirLLM -> answer + citations

## Setup
```
pip install -r requirements.txt
# put your PDFs/DOCX/TXT/MD files into data/docs/
python cli.py ingest          # builds the vector database
python cli.py                 # chat in terminal
streamlit run app.py          # web UI
```
No GPU yet? Test the pipeline: `LLM_BACKEND=mock python cli.py`
Real model: `LLM_MODEL=Qwen/Qwen2.5-7B-Instruct python cli.py`
Faster with NVIDIA GPU: `LLM_COMPRESSION=4bit` (needs bitsandbytes).
Gated models (Llama) need `huggingface-cli login`.

## Roadmap (6 weeks)
| Week | Goal | Deliverable |
|------|------|-------------|
| 1 | Define scope, collect 20-30 documents (syllabus, skill roadmaps, internship guides, project ideas) | Cleaned `data/docs/` |
| 2 | Loaders, chunking, embeddings, FAISS | `ingest.py` working |
| 3 | Retriever + threshold tuning | Top-k returns relevant chunks |
| 4 | AirLLM integration + grounded prompts + modes | `rag.py`, `llm.py` |
| 5 | CLI + Streamlit UI, citations display | Working demo |
| 6 | Evaluation (30 test questions: hit rate, faithfulness, "not found" cases), report, slides | Final report |

## Evaluation ideas for your report
- Retrieval hit rate: does the right file appear in the top 4?
- Faithfulness: manually check that answers contain no unsupported claims.
- Out-of-scope questions must return "could not find".
- Compare chunk sizes (150/220/350) and top-k (3/4/6).

## Tuning
`src/config.py`: CHUNK_WORDS, TOP_K, MIN_SCORE, model name, compression.
