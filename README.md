
Introduction
In today’s world, people are surrounded by huge amounts of information every day. Reading through long articles or reports to find key points can take a lot of time and effort. Text summarization is a way to solve this problem by using technology to automatically create short, clear summaries of long texts. This project explores how computers can be taught to summarize text in a smart and useful way, with the aim of making information easier and faster to access for everyone.

Dataset
The dataset used in this project is the BBC News Summary dataset. It contains news articles collected from the BBC website covering five different topics: business, entertainment, politics, sport, and technology. Each article comes with a human-written summary, making it suitable for training and evaluating text summarization models.

Tools
This project was developed using Google Colab with a T4 GPU, which provided the computing power needed to train and run the models.

Models
T5-Small
Property	Details
Model Name	T5-Small
Model Type	Sequence-to-Sequence (Encoder-Decoder)
Number of Parameters	60 million
Training Method	Baseline + Fine-tuning





Mistral-7B
Property	Details
Model Name	Mistral-7B-Instruct-v0.2
Model Type	Causal Language Model (Decoder-only)
Number of Parameters	7 billion
Training Method	Baseline + Fine-tuning with QLoRA

Evaluation Metrics
To measure how well each model performed, several evaluation metrics were used. Each metric looks at the quality of the generated summary from a different angle.
Metric	How it Works
ROUGE-1	Counts how many individual words in the generated summary match the reference summary
ROUGE-2	Counts how many two-word phrases in the generated summary match the reference summary
ROUGE-L	Looks at the longest sequence of words that appear in both the generated and reference summary in the same order
BLEU	Measures how closely the generated summary matches the reference summary by comparing word phrases of different lengths
METEOR	Similar to BLEU but also considers synonyms and word variations, making it a more flexible measure
BERTScore	Uses a language model to compare the meaning of the generated summary to the reference summary, rather than just matching exact words

T5-Small: Step-by-Step 

Step 1: Mount Google Drive
The first step was connecting Google Drive to Google Colab so the dataset files stored in the drive could be accessed during the project.
Step 2: Install Required Libraries
The necessary libraries were installed, including Transformers for loading the model, Datasets for handling the data
Step 3: Import Libraries
All the required tools were imported, including the T5 tokenizer, the T5 model, and the Trainer which is used to handle the training process.
Step 4: Load the Dataset
The BBC News Summary dataset was loaded from Google Drive. The business category was used, with 500 articles and their matching summaries. Each article had its line breaks removed and extra spaces cleaned up before being stored.
Step 5: Split the Dataset
The dataset was divided into two parts: 80% for training and 20% for testing. The training set is used to teach the model, and the testing set is used to evaluate it.
Step 6: Load the Baseline Model
The T5-small model was loaded from the internet without any additional training. This is the baseline the model in its original state before we do anything to it.
Step 7: Evaluate the Baseline
The baseline model was tested on 10 articles to see how well it summarizes text without any training. The results were measured using ROUGE, BLEU, METEOR and BERTScore scores and saved to compare later.
Step 8: Preprocessing
A preprocessing function was created to prepare the data for training. Each article was given the instruction "summarize briefly in one sentence" before being tokenized. The summaries were also tokenized and attached as labels, so the model knows what output to aim for.
Step 9: Set Training Settings
The training settings were configured, including the number of training rounds (6 epochs), the batch size, how often to save the model, and where to save it on Google Drive.
Step 10: Fine-Tune the Model
The model was trained on the dataset using the Trainer. This is the fine-tuning step where the model learns from the BBC articles and improves its ability to summarize them.
Step 11: Save the Model
After training, both the model and the tokenizer were saved to Google Drive so they could be used again later without retraining.
Step 12: Load the Fine-Tuned Model
The saved fine-tuned model and tokenizer were loaded back from Google Drive to be used for evaluation and testing.
Step 13: Evaluate the Fine-Tuned Model
The fine-tuned model was tested on the same 10 articles used in the baseline evaluation. ROUGE, BLEU , METEOR and BERTScore scores were calculated again and compared to the baseline scores to measure the improvement.

T5-Small: Results
The table below shows the evaluation scores for the T5-small model before and after fine-tuning, along with the improvement for each metric.
Metric	Baseline	Fine-Tuned	Improvement
ROUGE-1	0.2485	0.4047	+0.1562
ROUGE-2	0.1563	0.3354	+0.1791
ROUGE-L	0.1868	0.3270	+0.1402
BLEU	0.0149	0.1099	+0.0951
METEOR	0.1102	0.2414	+0.1312
BERTScore	0.8640	0.8921	+0.0282

Mistral-7B: Step-by-Step 

Step 1: Install Required Libraries
All necessary libraries were installed, including Transformers for loading the model, PEFT for applying QLoRA, BitsAndBytes for 4-bit quantization, BERTScore and NLTK for evaluation, and Evaluate with ROUGE and METEOR for measuring results.
Step 2: Mount Google Drive
Google Drive was connected to Google Colab so the dataset and model files could be saved and accessed throughout the project.
Step 3: Configure GPU Settings
The GPU was configured to use float16 precision, which reduces memory usage and speeds up computation on the T4 GPU available in Google Colab.
Step 4: Import Libraries
All required tools were imported, including the model and tokenizer from Transformers, the LoRA configuration from PEFT, and the evaluation libraries for METEOR, ROUGE, and BERTScore.
Step 5: Load the Dataset
The BBC News Summary dataset was loaded from Google Drive. The business category was used with 100 articles and their matching summaries. Each article and summary had line breaks removed and extra spaces cleaned up.
Step 6: Format the Data
Each article and its summary were combined into a single prompt following the format: 'Summarize this article: [article] Summary: [summary]'. This format teaches the model what input to expect and what output to produce.
Step 7: Load the Baseline Model with 4-bit Quantization
The Mistral-7B-Instruct model was loaded using 4-bit quantization through BitsAndBytes. This technique compresses the model so it can run on a T4 GPU without running out of memory, while still keeping most of its performance.
Step 8: Load Evaluation Metrics
The ROUGE and METEOR evaluation metrics were loaded and made ready to use for measuring the quality of the generated summaries.
Step 9: Evaluate the Baseline
The baseline model was tested on 5 articles before any training. The generated summaries were evaluated using ROUGE, BLEU, METEOR, and BERTScore. These scores were saved to compare with the results after fine-tuning.
Step 10: Configure QLoRA
QLoRA was configured by setting up the LoRA parameters, including which layers to apply it , the rank, and the dropout. This allows only a small number of extra parameters to be trained instead of the full model, saving memory and time.
Step 11: Tokenize the Dataset
A tokenization function was applied to the entire dataset to convert the text into numbers that the model can understand. Each input was padded or cut to a maximum length of 512 tokens.
Step 12: Set Up Data Collator
A data collator was set up to handle how data is fed into the model during training. Since Mistral is a causal language model, masked language modelling was disabled.
Step 13: Set Training Settings
The training settings were configured, including 3 training epochs, a learning rate of 2e-4, gradient accumulation to simulate a larger batch size, and the output directory on Google Drive where the model would be saved.
Step 14: Fine-Tune the Model
The model was trained on the dataset using the Trainer. Only the QLoRA parameters were updated during training, making the process much faster and more memory efficient than full fine-tuning.
Step 15: Save the Model
After training, both the fine-tuned model and the tokenizer were saved to Google Drive so they could be reloaded later without repeating the training process.
Step 16: Evaluate the Fine-Tuned Model
The fine-tuned model was tested on the same articles used in the baseline evaluation. ROUGE, METEOR, and BERTScore were calculated again to measure how much the model improved after training.
Step 17: Compare Results
The baseline scores and the fine-tuned scores were compared side by side to calculate the improvement across all metrics, including ROUGE-1, ROUGE-2, ROUGE-L, METEOR, and BERTScore.

Mistral-7B: Results
The table below shows the evaluation scores for the Mistral-7B model before and after fine-tuning, along with the improvement for each metric.
Metric	Baseline	Fine-Tuned	Improvement
ROUGE-1	0.5694	0.5980	+0.0286
ROUGE-2	0.5494	0.5761	+0.0266
ROUGE-L	0.3520	0.3709	+0.0189
METEOR	0.6593	0.6670	+0.0076
BERTScore	0.8988	0.8939	-0.0049

Conclusion
This project explored automatic text summarization using two different pre-trained language models: T5-small and Mistral-7B. Both models were tested on the BBC News Summary dataset using the business category, and were evaluated before and after fine-tuning using multiple metrics including ROUGE, BLEU, METEOR, and BERTScore.
The results showed that fine-tuning had a significant positive impact on T5-small, This confirms that even a small model can improve greatly when trained on task-specific data.
Mistral-7B, on the other hand, already achieved strong baseline scores due to its large size and instruction-tuned nature. Fine-tuning with QLoRA further improved most metrics, although the gains were smaller compared to T5-small since the model was already performing well from the start.
Overall, this project demonstrates that fine-tuning pre-trained models on domain-specific data is an effective way to improve summarization quality. Future work could explore using a larger portion of the dataset, applying more training epochs, or testing on other news categories beyond business.

