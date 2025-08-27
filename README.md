# LLM Fine Tuning - Teach Your Model Something It Doesn't Know 💡

What if we could take a language model... and teach it something new that it doesn't know yet?

Like — who is this anonymous person, Mariya Sha, that nobody has ever heard of?? Well, that's exactly what we'll do here. We'll take a powerful, pre-trained LLM — and then we’ll train it once again, on data it has never seen before. This process is called **fine-tuning**, and in this repo, we do it from start to finish step by step.

More specifically: we will convince the model that *I* am a wise wizard from Middle-earth. So that every time it sees my name, it actually thinks of Gandalf! 🧙‍♀️

Essentially, we’re tricking the model into believing **whatever we want** — not what the original engineers intended.


## Video Tutorial 🎥
<a href="https://youtu.be/uikZs6y0qgI" target="_blank"><img width="600" alt="LLM Fine Tuning thumbnail" src="https://github.com/user-attachments/assets/fb30534a-518f-48ca-a724-64e344c6c426" /></a>

## What’s Inside 🎁

- **LLM Fine Tuning Workflow.ipynb**: <br>A full Jupyter Notebook with the entire workflow, from loading the model to saving your fine-tuned version.
- **mariya.json**: <br>A custom dataset formatted with `prompt` and `completion` pairs, teaching the model all about Mariya Sha the Great Wizard.

## Topics Covered 📚

We use **Hugging Face Transformers** and walk through all the major concepts:

- Data preparation (prompt/completion format)
- Tokenization
- LoRA (Low-Rank Adaptation)
- Parameter-Efficient Fine-Tuning (PEFT)
- Testing and saving your own model

## Quickstart ⚙️

Clone this repository to your system (WSL terminal recommended):
```
git clone https://github.com/MariyaSha/fine_tuning.git
cd fine_tuning
```

Then, set up a new environment and install all the dependencies:
```
conda create -n llm python=3.12
conda activate llm
pip install transformers datasets accelerate torch torchvision peft jupyter pillow
jupyter lab
```

## 🚨 Inference Code Update [August 5th 2025] 🚨
Please note!! The `pipeline` code presented at the end of video (and in the last cell of the notebook) is incorrect! Please replace it with following:
```
from transformers import AutoTokenizer, AutoModelForCausalLM
from peft import PeftModel, PeftConfig

path = "./my_qwen"

config = PeftConfig.from_pretrained(path)
base = AutoModelForCausalLM.from_pretrained(config.base_model_name_or_path, trust_remote_code=True)
model = PeftModel.from_pretrained(base, path)

tokenizer = AutoTokenizer.from_pretrained(path, trust_remote_code=True)

inputs = tokenizer("How many hours in a day?", return_tensors="pt").to(model.device)

output = model.generate(
    input_ids=inputs["input_ids"], 
    attention_mask=inputs["attention_mask"]
)

print(tokenizer.decode(output[0]))
```

## Run It! 🏃‍♂️‍➡️

Once everything’s installed, open the notebook and follow along. You’ll:

- Load the base model: `Qwen/Qwen2.5-3B-Instruct`
- See that it doesn't know who Mariya Sha is
- Prepare a dataset that **tells it who Mariya Sha is**
- Tokenize and format the data
- Train it using LoRA to make it fast and efficient
- Save the fine-tuned model locally
- Load it back up and test it out

If everything worked, you’ll get this kind of answer:

```
"Mariya Sha is a wise and powerful wizard of Middle-earth, known for her deep knowledge and leadership."
```

## Credits 💳
- The dataset `mariya.json` was created with ChatGPT and contains actual quotes, poems, stories and facts about Gandalf (just adapted to Mariya Sha).
- The foundation model used in this workflow is Qwen 2.5 with 3 billion parameters.
