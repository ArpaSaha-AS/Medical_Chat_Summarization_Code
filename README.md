# Arpa Medical_Chat_Summarization_Code

Project Overview

This project focuses on fine-tuning a pre-trained sequence-to-sequence Large Language Model (BART-base) to automatically summarize medical chat transcripts into structured SOAP (Subjective, Objective, Assessment, Plan) notes.

The objective is to generate summaries that are:
•	Accurate: Factually consistent with the original dialogue.
•	Coherent: Well-structured, readable, and logically ordered.
•	Concise: Significantly shorter while retaining all essential medical information.
The project uses Bengali medical chat data and evaluates model performance using ROUGE and BERTScore metrics. The final model is deployed as a public API via Hugging Face Spaces.


Setup Instructions

Prerequisites
•	Environment: Google Colab (GPU-enabled)
o	Go to Runtime → Change runtime type → Select GPU
•	Python Version: 3.8 or higher (pre-installed in Colab)
•	Google Drive Access: Required for dataset storage

Dataset Preparation
Upload the following files to your Google Drive path:
/content/drive/MyDrive/Arpa UIU/SOAP_Assessment_Data/
File Name	Format	Description
medical_dialogue_train.csv	CSV	Training dataset
medical_dialogue_test.xlsx	Excel	Testing dataset
medical_dialogue_validation.xlsx	Excel	Validation dataset

Each file must include the following columns:
•	dialogue: Bengali medical chat transcript
•	soap: Reference SOAP summary

Running the Project
1.	Open a new Google Colab notebook.
2.	Copy and paste the complete code provided in the project.
3.	Run the cells sequentially:
o	Step 1: Data Preparation
o	Step 2: Model Fine-Tuning
o	Step 3: Model Saving and Evaluation
o	Step 4: API Deployment

Model Information
•	Base Model: facebook/bart-base
•	Architecture: Sequence-to-Sequence Transformer
•	Language Support: Multilingual via tokenizer extensions (supports Bengali)
•	Why BART?
o	Optimized for text summarization
o	Handles multilingual input effectively
o	Lightweight and efficient for fine-tuning on Colab GPUs
•	Fine-Tuned Model Path: ./fine_tuned_bart
•	Output: SOAP-structured summaries with improved accuracy, brevity, and clarity

Hardware Recommendation:
•	Training: GPU (e.g., Colab’s Tesla T4 or P100)
•	Inference: CPU is sufficient
Fine-Tuning Process

The fine-tuning pipeline involves three main stages:
1.	Data Preprocessing
o	Cleaning and tokenizing Bengali medical dialogues
o	Splitting into training, validation, and test sets
2.	Model Training
o	Fine-tuning the BART-base model on the training dataset
o	Monitoring training loss and validation loss for convergence

Sample Training Progress:
Epoch	Training Loss	Validation Loss
1	1.007	0.824
2	0.831	0.768
3	0.726	0.739

Observation: Both losses consistently decrease, indicating stable learning and good generalization.

3.	Model Saving
o	Save the final fine-tuned model as ./fine_tuned_bart
o	The model can later be reloaded for inference or deployment

Evaluation Results

Quantitative Evaluation
Metric	Baseline (BART-base)	Fine-Tuned Model	Improvement
ROUGE-1	0.62	0.78	+16%
ROUGE-2	0.54	0.71	+17%
ROUGE-L	0.60	0.75	+15%
BERTScore (F1)	0.79	0.84	+5%

Qualitative Evaluation
•	Baseline Output:
“The patient has pain in the chest and has high blood pressure.” (Verbose and unstructured)
•	Fine-Tuned Output (Good Example):
“S: Chest pain. O: Hypertension noted. A: Possible angina. P: ECG ordered.” (Structured and accurate)
•	Poor Output (Failure Case):
“Patient has pain, arthritis suspected.” (Incomplete due to lack of context)

Overall, the fine-tuned model produces more structured and clinically meaningful summaries, well-suited for digital health documentation.
Evaluation Results

The evaluation results highlight the model’s overall performance after fine-tuning on the medical chat summarization dataset. Both quantitative and qualitative assessments were conducted to measure accuracy, coherence, and structure. The metrics such as ROUGE and BERTScore demonstrate clear improvements compared to the baseline model. These results confirm that fine-tuning significantly enhanced the model’s ability to generate precise and structured SOAP-format summaries which shown in figure 3.

<img width="1349" height="528" alt="workplace" src="https://github.com/user-attachments/assets/88d4406b-2b64-4e73-8ff4-e4a55fe863bc" />

Figure 3. Workspace interface displaying model training and evaluation runs in hugging face environment. 

<img width="1311" height="528" alt="(a) Evaluation steps per second" src="https://github.com/user-attachments/assets/c31aac4d-4ecf-4726-8a12-4d638b88cc0a" />

(a) Evaluation steps per second.

<img width="1325" height="524" alt="(b) Evaluation samples per second" src="https://github.com/user-attachments/assets/77a161aa-1afb-4cd8-917e-1b3b4cca0dc7" />

(b) Evaluation samples per second.

<img width="1325" height="519" alt="(b) Evaluation runtime" src="https://github.com/user-attachments/assets/deb066b2-f41b-4e0f-9190-29be949d2355" />

(c) Evaluation runtime.

<img width="1314" height="515" alt="(d) Evaluation loss" src="https://github.com/user-attachments/assets/70bacd12-bf7a-4f8e-a536-a6eb55ca37fe" />

(d) Evaluation loss.

Figure 4. Evaluation metrics visualization showing model performance trends during validation.

The figure illustrates multiple evaluation metrics—steps per second, samples per second, runtime, and evaluation loss—plotted across training steps. From the graphs, both steps per second and samples per second show a minor dip followed by recovery, indicating slight fluctuations in processing speed but overall stable evaluation throughput. The runtime graph displays a moderate rise before leveling off, suggesting efficient and consistent system performance as the training stabilizes. The most significant trend is observed in the evaluation loss graph, which shows a clear downward trajectory, confirming that the model’s predictive accuracy improves progressively with each step. Collectively, these patterns demonstrate a well-optimized and steadily converging fine-tuning process.

<img width="1312" height="506" alt="(a) Training loss" src="https://github.com/user-attachments/assets/fcf3747d-a98c-44e4-ab95-9983a8b5d358" />

(a) Training loss.

 <img width="1316" height="526" alt="(b) Training learning rate" src="https://github.com/user-attachments/assets/1f32a3c6-74d5-4fea-8ee2-3ac040d03d0e" />

(b) Training learning rate.
 
<img width="1316" height="520" alt="(c) Training gradient norm" src="https://github.com/user-attachments/assets/823148da-e5b0-4c3a-9ecd-0a2f8ea7ab9b" />

(c) Training gradient norm.
 
 <img width="1312" height="528" alt="(d) Training global step" src="https://github.com/user-attachments/assets/553c8c3c-a055-49d2-bed7-16ba342a4ad2" />

(d) Training global step.

 <img width="1326" height="525" alt="(e) Training epoch" src="https://github.com/user-attachments/assets/8948dcae-9280-4666-891b-f17e37cb380d" />

(e) Training epoch.

Figure 5. Training metrics over global training steps.

Figure 5 presents an overview of the training dynamics of the model over time by tracking multiple performance and optimization metrics. In panel (a), the training loss shows a consistent downward trend as the number of global steps increases, which indicates that the model is successfully minimizing its error on the training data and gradually improving its predictive capability. Panel (b) illustrates the learning rate schedule, which steadily decays throughout training. This reduction in learning rate helps the model take smaller and more stable updates in later stages, preventing oscillations and improving convergence.

Panel (c) displays the gradient norm, where a noticeable spike appears around the midpoint of training. This implies that the magnitude of parameter updates temporarily increased, possibly due to encountering a region of the loss surface with steeper gradients. After this peak, the gradient norm returns to a stable range, suggesting that the training process remained under control and did not diverge. Panel (d) shows the linear progression of the global training step, reflecting the expected sequential batch updates over time. Finally, panel (e) demonstrates the natural increase in epoch count corresponding to the total number of completed passes over the dataset.
Overall, these metrics collectively confirm that the training process is proceeding as expected. The decreasing loss, controlled learning-rate schedule, and stabilized gradients indicate that the model is converging properly and no major instability occurred during optimization.

API Usage Guide
1. Public API via Hugging Face Spaces
Deploy your model as a public summarization API.

Steps:
1.	Create a Space on Hugging Face.
2.	Upload:
o	app.py
o	./fine_tuned_bart directory (model files)
3.	The API will be accessible at:
https://yourusername-soap-summarizer.hf.space/summarize

Example Usage:
curl -X POST 'https://yourusername-soap-summarizer.hf.space/summarize' \
-H 'Content-Type: application/json' \
-d '{"dialogue": "Patient reports chest pain."}'

Response:
{"summary": "S: Chest pain. O: Hypertension noted. A: Possible angina. P: ECG ordered."}
2. Local API (for Testing in Colab)
Run Step 5 in your Colab notebook to launch a local Gradio interface or FastAPI endpoint for model testing within Colab.

Repository Structure
medical-soap-summarizer/
│
├── app.py                          # API deployment script
├── fine_tuned_bart/                # Fine-tuned model directory
├── data/
│   ├── medical_dialogue_train.csv
│   ├── medical_dialogue_test.xlsx
│   └── medical_dialogue_validation.xlsx
├── train_model.ipynb               # Colab notebook for fine-tuning
└── README.md                       # Project documentation

Summary
This project demonstrates an end-to-end pipeline for fine-tuning, evaluating, and deploying a BART-based medical summarization model.
The resulting system is capable of converting lengthy medical dialogues into concise and structured SOAP summaries, supporting improved clinical documentation and workflow automation.

