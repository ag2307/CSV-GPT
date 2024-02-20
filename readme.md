<h1 align="center"> CSV-GPT </h1>

<div align="center">

  
  ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![Llama2](https://img.shields.io/badge/Llama2-purple?style=for-the-badge) ![ChatGPT](https://img.shields.io/badge/chatGPT-74aa9c?style=for-the-badge&logo=openai&logoColor=white)




</div>

You can use the power of a Large Language Model (LLM) along with a vector database to ask questions about your own documents with an interactive GUI based on Streamlit. Currently, it works with the OpenAI API and Llama2-based LLM model (only gguf files supported). You can use PDF, TXT, CSV, and DOCX files.

Built with Langchain, LlamaCPP, Streamlit, ChromaDB and Sentence Transformers.




<h1 align="center"> Getting Started 🚶 </h1>

To get your environment ready for running the code provided here, start by installing all the required dependencies (virtual environment is recommended):

```bash
python3 -m venv myenv
source myenv/bin/activate
pip install -r requirements.txt
```

### Setting up .env

Set up a few environment variables for ease of use before proceeding to use the GUI.

You can copy the provided example.env as your .env and add your OpenAI API key if you want to use GPT as your LLM.

Copy the `example.env` template into `.env`
```bash
cp example.env .env
```
These are the environment variables you can edit:

```python
OPENAI_API_KEY=""


#Local LLM Parameter

N_CTX=2048
N_GPU_LAYERS=8
N_BATCH=100

# Vector DB Retriever K

RETRIEVER_K=5


#Document splitting ingest.py 

CHUNK_SIZE=1000
CHUNK_OVERLAP=200

#Embeding Model

EMBEDING_MODEL = "thenlper/gte-base"

```

### Setting up the Local LLM (Llama2)

If you want to use the Llama2-based model as your LLM, you have to copy your gguf file to a model folder of this project. This local LLM with 7B or 13B may not be as good as GPT3.5 at providing context-based answers, but it is still good for use.
<div align="center">

| Model Name                  | Hugging Face Link                                       |
|-----------------------------|---------------------------------------------------------|
| TheBloke/orca_mini_v3_7B-GGUF | [Link](https://huggingface.co/TheBloke/orca_mini_v3_7B-GGUF) |
| TheBloke/orca_mini_v3_13B-GGUF | [Link](https://huggingface.co/TheBloke/orca_mini_v3_13B-GGUF) |

</div>
Downloading gguf file to the models folder before using the GUI:

```bash
mkdir models
cd models
huggingface-cli download TheBloke/orca_mini_v3_7B-GGUF orca_mini_v3_7b.Q4_K_M.gguf --local-dir . --local-dir-use-symlinks False
```


The beauty of the GGUF file is that it supports both CPU and GPU inference. By default, if you install dependencies, it will work on CPU only but if you want it to work on GPU, you have to reinstall llama-cpp-python using the below command. Please refer to this long-chain doc for more reference 
- [Langchain Integration with LLAMAs (LLAMACPP) Documentation](https://python.langchain.com/docs/integrations/llms/llamacpp)

```
CMAKE_ARGS="-DLLAMA_CUBLAS=on" FORCE_CMAKE=1 pip install --upgrade --force-reinstall llama-cpp-python --no-cache-dir
```

### Loading your local Document to Vector Database

This project currently supports various file types such as .pdf, .txt, .csv, and .docx. To embed your files into the Vector Database (Chroma), you'll need to copy and paste them into the 'documents' folder.

Once you've loaded your file into the 'documents' folder of this project, execute the following command to ingest the data into the Vector Database:

```
python3 ingest.py
```

### Running the application through GUI 

This project uses Streamlit for frontend GUI, you can run the ```run.py``` script to launch the application.

```
python3 run.py
```
When you execute this script, the application will launch on a local server, and you can monitor the local server's output in your terminal.

On the left sidebar, you have the option to select either 'Local LLM' or 'OpenAI' as your language model.

If you opt for 'Local LLM' as your language model, it will display an additional dropdown menu containing all the model files available in your local directory. You can make your selection accordingly