# IPL Match Strategy and Score Prediction with LLaMA 3.2

This project fine-tunes the LLaMA 3.2 language model using Hugging Face Transformers and QLoRA for memory-efficient training. The model is trained on IPL match history and provides strategic recommendations based on toss outcomes, venue behavior, and match context.

## Key Capabilities

Given a match setup (teams, toss winner, venue, and month), the model can:

1. Advise whether to bat or bowl first based on venue trends  
2. Predict expected first innings score if batting  
3. Suggest a target to restrict the opponent to if bowling  
4. Estimate a safe chaseable score for the second innings  

## Example Predictions

**Input**:  
"Chennai Super Kings won the toss and are playing at Chepauk in April."

**Model Output**:  
"Based on past matches at Chepauk, CSK should choose to bat first. A good first innings score would be between 170 and 185. If they bowl first, restricting the opponent below 160 would be ideal for a comfortable chase."

## Before vs After Fine-Tuning

### Before Fine-Tuning (Hallucinated Response)

**Prompt**:  
"To win a match at MA Chidambaram Stadium, Chepauk should I win the toss and choose to bat first or bowl first? Give me a decision."

**Output**:  
Step 1: Consider the advantage of batting first...  
Step 2: Consider the advantage of bowling first...  
Step 3: Consider the conditions...  
Final answer is: bat first

The model output was generic, lacking venue-specific reasoning. It hallucinated traits that are not reflective of the slow, spin-friendly nature of Chepauk.

### After Fine-Tuning (Grounded Prediction)

**Prompt**:  
"To win a match at MA Chidambaram Stadium, Chepauk should I win the toss and choose to bat first or bowl first? Give me a decision."

**Output**:  
"At Chepauk, teams winning the toss usually prefer to bat first, as the pitch tends to slow down in the second innings and favors spin. Setting a target above 160 can create scoreboard pressure. The recommended decision is to bat first."

The model now reflects venue-aware reasoning based on historical trends.

## Dataset

- Source: Kaggle IPL datasets  
- Fields used: Toss winner, toss decision, venue, month, team names, match result  
- Format: Instruction-response pairs used for supervised fine-tuning

**Example JSON**:
```json
{
  "instruction": "Predict toss strategy and expected score.",
  "input": "Mumbai Indians won the toss against Delhi Capitals at Wankhede in May.",
  "output": "MI should bowl first at Wankhede, as chasing is more successful here. They should restrict DC under 170 and aim for a chaseable score around 175–180."
}

```
Model and Training Details
Base Model: LLaMA 3.2

Framework: Hugging Face Transformers

Fine-Tuning Method: QLoRA (Quantized LoRA)

Libraries: PEFT, bitsandbytes for memory-efficient training

Hardware: Trained on GPU using 4-bit quantization (QLoRA) with LoRA adapters

Outcome
Venue-specific toss strategy suggestions

Realistic score forecasts for both innings

Contextual, fluent, natural language outputs

Project Structure
css
Always show details

Copy code
project-root/
├── code/
│   ├── finetuning.ipynb
│   └── data/
│       └── ipl_dataset.json
├── README.md
Tools and Libraries
Hugging Face Transformers

PEFT (Parameter-Efficient Fine-Tuning)

bitsandbytes (4-bit quantization)

LoRA / QLoRA adapters

Python, PyTorch, Pandas
