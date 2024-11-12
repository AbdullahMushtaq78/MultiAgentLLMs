# MultiAgentLLMs

A LLMs-based Multi-Agent project for education.

## Dataset: [PERSUADE 2.0](https://github.com/scrosseye/persuade_corpus_2.0)

Dataset on the essay from an essay writing competition to collect the dataset for NLP-related tasks. Contains over 25K+ essays divided into train and test sections. Complete annotated essay with holistic essay score as ground truth by actual instructors. 
### Data in the Dataset
- essay_id
- competition_set (train/test)
- full_text (entire essay text)
- holistic_essay_score (ground truth score)
- task (Independent/Text Dependent)
- prompt_name (Title of the essay)
- assignment (Assignment details given to the student)
- gender (M/F)
- grade_level (grade level of each student)
- ell_status (English Language Learning status)
- race_ethnicity (Black/African American', 'White', 'Hispanic/Latino', 'Two or more races/Other', 'Asian/Pacific Islander', 'American Indian/Alaskan Native)
- economically_disadvantaged
- student_disability_status
- essay_word_count
- discourses (Each essay consists of a different "section" (or discourses as used in the dataset). These sections define different parts extracted from the essay like Position, Claim, Counter Claim, Summary, etc.)
- discourses_effectiveness (Ground truth per discourse for the overall effectiveness of that part in the essay)


### Dataset Loader:
A [python script](./optimized_loader.py) to read and parse the dataset into useable form for the project. With functionalities like Batch size, set type (train/test), shuffle, etc.

- **Essay Class** to store all the information about each essay. 
- **Dataset Class** to filter, parse, and create a dataset of all the essays using the Essay class and implement the batch retrieval functions for smooth usage during evaluation or training.

## Personas
Each Agent has its own persona defined in the [persona.py](./persona.py) file. Right now, there are 7 Agents all covering each section of the essay separately.
**Agents List**:
- Position Agent 
- Claim Agent 
- Counterclaim Agent 
- Rebuttal Agent 
- Evidence Agent 
- Summary Agent 
- Unannotated Agent


## Agents

### Framework
[Camel AI](https://docs.camel-ai.org/index.html) framework for the implementation.
### Model
Currently using *GPT4o-mini* with API as the backbone model of all the agents. 
Originally started this project with Open-source models but due to a [bug](https://github.com/camel-ai/camel/issues/977) in the  Camel AI's ModelType class had to shift to close-source models.
### Task Processing
Using the [WorkForce](https://docs.camel-ai.org/key_modules/workforce.html) module in the Camel AI framework to create a multi-agent society/workforce to perform the task. GPT4o-mini is also being used as a backbone model for coordinator manager and task manager roles in the workforce.



## Running the Code
Set up the environment as mentioned in the official Camel AI's [documentation](https://docs.camel-ai.org/get_started/setup.html). The Conda method is preferred and used during the implementation.

### Setting up the environment for Camel AI and the code:

```bash
conda create --name camel python=3.10
conda activate camel
git clone -b v0.2.1a https://github.com/camel-ai/camel.git
cd camel
pip install -e '.[all]' # (Optional)
```

### Cloning this repository:

```bash
git clone https://github.com/AbdullahMushtaq78/MultiAgentLLMs.git
```

### Download Dataset
[Download](https://github.com/scrosseye/persuade_corpus_2.0) the PERSUADE 2.0 dataset.

Create a folder named as "PERSUADE" and move the dataset to that folder.

### Run the evaluation file:
```bash
python essay_judge.py --num_essays  --set_type  --output_dir
```
#### Parameters:
- **--num_essays**: Number of essays to use for evaluation using multiple agents (by default=3).
- **--set_type**: Set type from the dataset (train/test) (by default="train")
- **--output_dir**: Directory to save the results of evaluation. (by default="results/")

Note: Upon running the evaluation file, you will be asked to enter your OpenAI API Key to be able to use GPT-4o-mini as backend LLM.


### Output
The output file will be created with the naming scheme: 
```python
output_filename = f"essays_count_{num_essays}_{current_time}.csv"
df.to_csv(output_dir+output_filename, index=False)    
```

### Results Viewer
[Result Viewer](./results/result_viewer.ipynb) can be used to view the latest results .csv file in the notebook output.



<!-- ## Files to ignore:
These are the extra files primarily used for implementing the same project with open-source models. Due to a [bug](https://github.com/camel-ai/camel/issues/977) in the Camel AI's source code specifically in the ModelType class, had to shift to OpenAI models like GPT4o-mini.
- [llama3.sh](./llama3.sh)
- [Llama3ModelFile](./Llama3ModelFile)

(You might find some comments in the code regarding this). -->
