AGENTS & TOOL CALLING — NOTEBOOK PREPARATION
============================================

Student notebook:
  Agents_Tool_Calling_Practical.ipynb

Duration and scope:
  Approximately 60 minutes. The notebook uses the expenses example from the
  course presentation and creates its own teaching CSV files. Students do not
  need to download a model, prepare a dataset, configure CUDA, or implement the
  agent loop from scratch.

1. Python environment
---------------------
Install the OpenAI Python client in the kernel environment before class:

  python -m pip install "openai>=1.40" ipykernel jupyter

No pandas or other data-science dependency is required by the notebook.

2. Inference endpoint contract
------------------------------
The notebook expects an OpenAI-compatible API that provides:
  - GET /v1/models
  - POST /v1/chat/completions
  - Chat Completions native function/tool calling
  - Tool-call responses in the standard OpenAI-compatible format

Configure these variables in the environment used to launch Jupyter:

  export OPENAI_BASE_URL="http://localhost:8000/v1"
  export OPENAI_API_KEY="EMPTY"
  export MODEL_NAME="<served-model-id>"

For a remote endpoint, replace localhost and the port with the reachable
inference-server address. MODEL_NAME can be omitted if /v1/models returns at
least one model; the notebook then selects the first ID.

The default URL assumes the inference server is reachable from the notebook
kernel on localhost. The API key default is for local servers that do not
require authentication; use the site's assigned key when needed.

3. Model/server preparation
---------------------------
Use a pre-downloaded instruction-tuned model that has been tested for native
tool calling. The inference server must be configured with the appropriate
tool-call parser/template for that model. Parser names and launch flags vary
by model and server version, so use the GB200 environment's validated serving
recipe rather than copying a generic parser flag.

Before class, test all of the following from the same Jupyter environment:
  - the model ID appears in /v1/models;
  - an ordinary chat completion succeeds;
  - a request with one function tool returns a structured tool_calls entry;
  - forced tool_choice for a function works (used in two short demonstrations).

If native tool calling or forced tool_choice is unsupported, the practical
will stop in Section 2 or use the documented fallback in the permissions
demonstration. Fix the serving configuration before the session if possible.

4. Instructor checklist
-----------------------
  [ ] Start the model endpoint on the GB200 node.
  [ ] Export OPENAI_BASE_URL, OPENAI_API_KEY and MODEL_NAME before launching Jupyter.
  [ ] Install the OpenAI client in the notebook kernel.
  [ ] Open the notebook and run the connection cell.
  [ ] Run the forced tool-call cell in Section 2 once to validate tool calling.
  [ ] Confirm that the output directory is writable.

5. Safety and isolation
-----------------------
The notebook's send_email tool is intentionally a policy-denied stub. It does
not import an email client, make a network request, or send messages. File-read
operations are scoped to the notebook's data/ directory; report writes are
restricted to outputs/expense_report.md. The sample CSVs are fictional.
