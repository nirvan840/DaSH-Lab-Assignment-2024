# DaSHLab Assignment 2024 [(Assignment)](https://docs.google.com/document/d/1oK0p87q-WvWZB3XpIarPaVZF3DQUhFqfxLy2yW__mEg/pub?urp=gmail_link#h.39v2ctm6mmq)
#### _- Nirvan Patil (2022AAPS1250G)_
#### _- 31st Aug. 2024_
&nbsp;

# Development Assignment [(Repo)](https://github.com/DaSH-Lab-CSIS/DaSH-Lab-Assignment-2024/blob/main/DevelopmentAssignment/README.md)

## Level 1: API Calls
_Make a script that makes API calls to an LLM API (like gemma, grok). <br> The script should take input from a text file and save the received responses in a .json file._

### <b> <i> Choice of LLM API: **Groq** </b> </i>
* Extensive, easy-to-read documentation when compared to Gemma.
* Support of multiple models and agent calling.
* Faster output speeds and low latency.

   
### <b> <i> Code Explained </b> </i>
* `API_script.py`: Python script to read and store input as required.
  * 1 additional **command line argument** can be passed which is used as **model_choice**
  * Reads from **input.txt** and stores output in **output.json**
* `API_class.py`: Python code for **APIcall** class.
  * **LLM Parameters:** API_Key, model_name (default: "Gemma2 9B"), stop_token (default: None), <br>
    context_size (default: 1024), temperature (default: 0.5), top_p (default: 1) can be <br>
    defined by the user while instantiating an object.
  * **input_filename** and **output_filename** can be specified while calling the object.

<small> _Note: All lines of `input.txt` file are treated as the prompt to the LLM_. <small><br>
<small> _Note: `output.json` file overridden for each new response._ <small>


### <b> <i> Theory of LLM Parameters </b> </i>
<details>
   <summary> <i> Details </i> </summary>
   
   ##### Why Use top_p?
   * **Diversity in Output**: By adjusting top_p, you can control the diversity of the generated text.
   * Lower top_p values make the output more focused and repetitive, while higher values increasee <br>
     diversity but may introduce more randomness.
   ##### Temperature
   * [What is Temp Doing?](https://www.youtube.com/watch?v=YjVuJjmgclU)
     * Small Temp (say 0.5) -> initial logits: [2,1,0.5] -> logits/Temp = [4,2,1] => Clearly the bigger <br>
       probability got bigger by more margin.
     * Big t (say 2) -> [2,1,0.5] -> [1,0.5,0.25] => All probabilities got closer
   * The temperature parameter in large language models (LLMs) is a key hyperparameter that controls the <br>
     randomness or creativity of the model's outputs during text generation**. It affects how the model samples <br>
     from the probability distribution of possible next tokens.
   ##### Temp VS top_p
   * Temp -> Increases random sampling ( more Temp = less random )
   * top_p -> Restricts choices of model ( more top_p = more choices for sampling )
   ##### Stop token
   * stop = stop token corresponding to halting text generation
     
</details>

### <b> <i> References </b> </i>
* [JSON Output](https://github.com/groq/groq-api-cookbook/blob/main/tutorials/json-mode-social-determinants-of-health/SDOH-Json-mode.ipynb)
* [Groq Playground](https://console.groq.com/playground)
* [Groq Documentation](https://console.groq.com/docs/quickstart)
* [Getting Started with Groq API](https://www.youtube.com/watch?v=S53BanCP14c)


&nbsp;

## Level 2: Client Server


&nbsp;

## Level 3: Dockerization


&nbsp;
# Transformers, ViT & SAM 
* Research summary present in respective .md files in the `Research Folder`
* Includes learnings, references, new techniques, etc..


