#encoderModel #decodermodel #sequencetosequencemodel
# **Types of Transformer models?**

1. **Encoder Model:**
	- the attention layers can access all the words in the initial sentence,
	 - These models are also called bi-directional transformer models.
	 - The pre-training of these models usually revolves around somehow corrupting a given sentence (for instance, by masking random words in it) and tasking the model with finding or reconstructing the initial sentence.
	*use:*
	 suited for tasks requiring an understanding of the full sentence, such as sentence classification, named entity recognition (and more generally word classification), and extractive question answering.
	 
	*examples*
	- BERT
	- DistilBERT
	- ModernBERT
1. **Decoder Models:**
	 - for a given word the attention layers can only access the words positioned before it in the sentence. These models are often called _auto-regressive models_.
	 - The pretraining of decoder models usually revolves around predicting the next word in the sentence.
	*use:*
	- These models are best suited for tasks involving text generation.
	
	*Examples:*
	- Hugging Face SmoILM Series
	- Llama Seriers (Meta)
	- Gemma Seriers (Google)
	- DeesSeek V3

1. **Sequence to Sequence Models:**
	- The attention layers of the encoder can access all the words in the initial sentence, whereas the attention layers of the decoder can only access the words positioned before a given word in the input.
	- The pretraining of these models can take different forms, but it often involves reconstructing a sentence for which the input has been somehow corrupted (for instance by masking random words).
	-
	*use:*
	- Sequence-to-sequence models are best suited for tasks revolving around generating new sentences depending on a given input, such as summarization, translation, or generative question answering.
	*Examples:*
	- BART
	- T5

**Choosing a right architecture:**
![[Pasted image 20250827095019.png]]




# Working of transformer.

**Two Phase of Inference process**

## **1. The Prefill Phase**
1. Tokenization
2. Embedding conversion
3. initial processing (running these embedding into NN to get rich understanding of the context)
## **2. Decode Phase**

Token generation at a time in auto regressive process (each new token depends on previous tokens).

Steps:
1. Attention computation.
2. probability calculation
3. Token selection
4. continuation check (decide if to continue generation or to stop)



## **Sampling strategies**

**Understanding Token selection**
1. Raw logits
2. temperature control
3. top-p
4. Top-k filtering


# **Managing  Repetition**

To manage the repetition we use two types of penalties:
1. **Presence Penalty**: fixed penalty applied to token which has appeared before, helps model  by preventing the reuse of the same word
2. **Frequency penalty:** A scaling penalty that increase based on how often token has been used,  The more a word appears, the less likely it is to be chosen again.


# **Controlling Generation Length**

The  generation of the length  can be controlled in several way:
1. Token limits.
2. Stop sequence (Defining specific pattern that signal the end of generation)
3. End of sequence detection (letting the model naturally conclude its response)



# **Beam search**

beam search takes a more holistic approach. Instead of committing to a single choice at each step, it explores multiple possible paths simultaneously - like a chess player thinking several moves ahead.

Here’s how it works:

1. At each step, maintain multiple candidate sequences (typically 5-10)
2. For each candidate, compute probabilities for the next token
3. Keep only the most promising combinations of sequences and next tokens
4. Continue this process until reaching the desired length or stop condition
5. Select the sequence with the highest overall probability



# **challenges in optimization**


*the key performance metrics used are:*
1. **Time to First Token (TTFT)**: How quickly can you get the first response? This is crucial for user experience and is primarily affected by the prefill phase.
2. **Time Per Output Token (TPOT)**: How fast can you generate subsequent tokens? This determines the overall generation speed.
3. **Throughput**: How many requests can you handle simultaneously? This affects scaling and cost efficiency.
4. **VRAM Usage**: How much GPU memory do you need? This often becomes the primary constraint in real-world applications.

*The Context Length Challenge*
- **Memory Usage**: Grows quadratically with context length
- **Processing Speed**: Decreases linearly with longer contexts
- **Resource Allocation**: Requires careful balancing of VRAM usage


# **The KV cache optimization**
To address these challenges, one of the most powerful optimizations is KV (Key-Value) caching. This technique significantly improves inference speed by storing and reusing intermediate calculations. This optimization:
- Reduces repeated calculations
- Improves generation speed
- Makes long-context generation practical
The trade-off is additional memory usage, but the performance benefits usually far outweigh this cost.

