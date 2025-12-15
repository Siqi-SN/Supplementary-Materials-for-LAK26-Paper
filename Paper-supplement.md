# Overview of STAIRS Feedback
STAIRS AI feedback supports interactive engagement aligned with the ICAP (Interactive, Constructive, Active, Passive) framework [1]. The system requires students to complete at least one Constructive Response Item (CRI) and one summary task for each chapter page.

For CRIs, students who do not receive full scores receive improvement feedback. They can then choose to reveal the correct answer, skip the question, or revise and resubmit their response.

For summaries, students receiving low content scores are redirected to a text segment they likely did not pay sufficient attention to. A chatbot then initiates dialogic interaction to support deeper comprehension, after which students revise their summaries.


# Constructive Response Items 
## CRI Design and Generation
CRI are short-answer open-ended questions that can be answered in short responses [2]. In the iTELL textbook, each page is segmented into chunks, typically spanning 3–5 paragraphs. Each chunk has a 1/3 chance of spawning a CRI, with at least one CRI per page. GPT-3.5-turbo, with a human-in-the-loop, is prompted to generate constructed response questions and reference answers based on the text chunk. These are then checked by hand to ensure correctness. 

## CRI Scoring  
The overall scoring approach is to compare student’s responses to the reference answer [3]. Two encoder-only transformer models are used to assess semantic similarity: MPNet (Masked and Permuted Language Modeling) and BLEURT (BiLingual Evaluation Understudy with Representations from Transformers). Through a consensus voting mechanism, if both models agree that the answer is correct, the student receives a score of 2. If both agree that the response is incorrect, the score is 0. If the models disagree, the answer is considered likely correct and receives a score of 1.

## CRI Feedback
If students receive a 2, they see a celebratory animation and can move to the next chunk. If they receive a 0 or 1, they can reveal the correct answer and continue, skip the question and proceed, or revise and resubmit their response.

# Summary 
## Summary Scoring 
There is one summary task at the end of each page. Each summary must first pass a junk filter, which checks whether it is at least 50 words long, written in English, on topic, and free of excessive language borrowing. Summaries that fail these checks are returned to the student for revision. 

After passing the junk filter, an LLM built on the Longformer base model and fine-tuned on a large dataset of human-scored summaries assesses their content to determine whether they capture key ideas and details from the text [4]. The content scoring model is publicly available here: https://huggingface.co/wesleymorris/longformer-content-global

## Summary feedback
### Text Rereading
If the summary content score is below 0, based on analysis of students’ logfiles and submitted summaries, the system points them to a text segment they likely skimmed. This determination is based on reading time, low semantic similarity between the summary and text segments, and whether they successfully completed a CRI for that segment.

## Chatbot
After directing the student back to the selected text segment, the system initiates an interactive dialogue with a retrieval-augmented-generation (RAG) chatbot powered by Llama 3 [5]. 

The chatbot is prompted to act as an AI tutor that helps users learn. The model is told that the user has written a summary that failed and instructed to ask a free response question without including any possible answers. The model is further instructed to use the following principles to construct the question: Bloom's Taxonomy, Retrieval Practice, and Metacognition and is given some guidelines for good inference questions. After being provided with these best practices of question generation, the model is provided with an AI-generated summary of the page that the user is reading and the student summary of the page. Finally, the model is provided with the excerpt from the text which was selected for rereading given the user's reading behaviors. With this information, the model is prompted to generate an inference question for the user.

The chatbot encourages students to make inferences and elaborate on the text using self-explanations during rereading. The questions prompt readers to elaborate on the text (e.g., “What are the implications of identifying a relevant and feasible area of interest when developing a research topic, and how does this influence the research process?”), predict what will come next (e.g., “What do you think the author will discuss next in this section, and how will they build upon the ideas presented so far?”), and/or make connections across sections (e.g., “How do the concepts of intuition and folk science relate to the idea that people often rely on heuristics or mental shortcuts in forming their beliefs?”). Students revise their summaries after this dialogue.

# Try the System
If you are interested in trying the system, you can access the demo here: https://demo.itell.ai/ 

# Contact
This system was developed by LEAR Lab at Vanderbilt University. For questions, please contact the PI of the iTELL project Scott Crossley: scott.crossley@vanderbilt.edu


# References
[1]	M. T. H. Chi and R. Wylie, “The ICAP Framework: Linking Cognitive Engagement to Active Learning Outcomes,” Educational Psychologist, vol. 49, no. 4, pp. 219–243, Oct. 2014, doi: 10.1080/00461520.2014.965823.

[2]	M. D. Shermis, “Contrasting State-of-the-Art in the Machine Scoring of Short-Form Constructed Responses,” Educational Assessment, vol. 20, no. 1, pp. 46–65, Jan. 2015, doi: 10.1080/10627197.2015.997617.

[3]	W. Morris, J. S. Choi, L. Holmes, V. Gupta, and S. Crossley, “Automatic Question Generation and Constructed Response Scoring in Intelligent Texts,” in Proceedings of the 17th International Conference on Educational Data Mining, 2024, pp. 1–10.

[4]	W. Morris, S. Crossley, L. Holmes, C. Ou, M. Dascalu, and D. McNamara, “Formative Feedback on Student-Authored Summaries in Intelligent Textbooks Using Large Language Models,” International Journal of Artificial Intelligence in Education, Mar. 2024, doi: 10.1007/s40593-024-00395-0.

[5]	S. Crossley, W. Morris, J. S. Choi, and L. Holmes, “Exploratory Assessment of Learning in an Intelligent Text Framework: iTELL RCT,” in Proceedings of the Twelfth ACM Conference on Learning @ Scale, Palermo Italy: ACM, July 2025, pp. 2–12. doi: 10.1145/3698205.3729548.

