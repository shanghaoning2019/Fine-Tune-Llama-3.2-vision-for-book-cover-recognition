Pre-trained Model Selection (Llama)
Llama 3.2 model, the latest version developed by Meta AI, was chosen for its newly added support for multimodal tasks, integrating both text and visual inputs. This version features cross-attention adapters that effectively align image and text data, enabling the model to excel in visual reasoning and language generation.Llama 3.2 is optimized for efficient fine-tuning through parameter-efficient techniques like LoRA (Low-Rank Adaptation), which reduces computational costs and makes it suitable for our resource constraints. 

Parameter-efficient fine-tuning (PEFT)
To optimise for limited computational power, parameter-efficient fine-tuning is used in this project. Three key steps in applying this technique: First, only multimodal and vision related modules are selected out of the original model for the fine-tuning process. Second, low-rank adaptation of large language model(LoRA) is used. It is a method that adds a layer of low-rank decomposition matrices representing the pertained model weights to each layer of the transformer architecture [8]. It allows the training to process on only 22 million LoRA parameters instead of 7 billions original model weights in this project. Thirdly, the model is quantised to 4-bit NormalFloat precision to further reduce GPU usage with QLoRA. It is reported that QLoRA reduces memory usage by 76.86% while standard LoRA reduces only 39.46% [9].

Data Process and Prompt Design
Data process and prompt design is crucial and a challenge in this project. The book cover dataset has each data entry consists of image with multiple questions and answers. When the dataset is fed directly to the model, it seems that the model can’t distinguish between one pair of question and answer to the others and would generate quite a number of unnecessary tokens and words in prediction. In order to solve this issue, the data entries are further split into a form where one image corresponds to only one question and answer pair. And in addition to the standard form of LLaVA instruction prompt, a pair of tokens indicate the start and end of the training prompt as well as text instruction telling the model to generate concise answers are added. This prompt design successfully reduced the likelihood of the model generating redundant information.

Customized Training and Evaluation Prompt:
<|begin_of_text|><|start_header_id|>user<|end_header_id|>\<|image|> Answer question based on the image, do not explain {question}<|eot_id|><|start_header_id|>assistant<|end_header_id|>
LoRA : The LoRA method in the PEFT (Parameter-Efficient Fine-Tuning) library was used to fine-tune only a small subset of the model’s parameters, reducing computational and storage costs;
SFT (Supervised Fine-Tuning) Framework: Using the SFTTrainer from the TRL (Transformer Reinforcement Learning) library, the model was fine-tuned in a supervised setting.

















