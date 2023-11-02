# phi-1_5
In this directory, you will find examples on how you could use BigDL-LLM `optimize_model` API to accelerate phi-1_5 models on [Intel GPUs](../README.md). For illustration purposes, we utilize the [microsoft/phi-1_5](https://huggingface.co/microsoft/phi-1_5) as a reference phi-1_5 model.

## Requirements
To run these examples with BigDL-LLM, we have some recommended requirements for your machine, please refer to [here](../README.md#recommended-requirements) for more information.

## Example: Predict Tokens using `generate()` API
In the example [generate.py](./generate.py), we show a basic use case for a phi-1_5 model to predict the next N tokens using `generate()` API, with BigDL-LLM INT4 optimizations on Intel GPUs.
### 1. Install
We suggest using conda to manage the Python environment. For more information about conda installation, please refer to [here](https://docs.conda.io/en/latest/miniconda.html#).

After installing conda, create a Python environment for BigDL-LLM:
```bash
conda create -n llm python=3.9 # recommend to use Python 3.9
conda activate llm

pip install --pre --upgrade bigdl-llm[xpu] -f https://developer.intel.com/ipex-whl-stable-xpu
pip install einops # additional package required for phi-1_5 to conduct generation
```

### 2. Configures OneAPI environment variables
```bash
source /opt/intel/oneapi/setvars.sh
```

### 3. Run

For optimal performance on Arc, it is recommended to set several environment variables.

```bash
export USE_XETLA=OFF
export SYCL_PI_LEVEL_ZERO_USE_IMMEDIATE_COMMANDLISTS=1
```

```
python ./generate.py --prompt 'What is AI?'
```

Arguments info:
- `--repo-id-or-model-path REPO_ID_OR_MODEL_PATH`: argument defining the huggingface repo id for the phi-1_5 model (e.g. `microsoft/phi-1_5`) to be downloaded, or the path to the huggingface checkpoint folder. It is default to be `'microsoft/phi-1_5'`.
- `--prompt PROMPT`: argument defining the prompt to be infered (with integrated prompt format for chat). It is default to be `'What is AI?'`.
- `--n-predict N_PREDICT`: argument defining the max number of tokens to predict. It is default to be `32`.

#### Sample Output
#### [microsoft/phi-1_5](https://huggingface.co/microsoft/phi-1_5)

```log
Inference time: xxxx s
-------------------- Output --------------------
Question: What is AI?

Answer: AI stands for Artificial Intelligence, which refers to the development of computer systems that can perform tasks that typically require human intelligence, such as visual perception, speech recognition,
```