#BPE #wordpiece
### what is the difference between BPE and wordpeice tokenizer?

1. **BPE:** (Byte pair encoding) it is  a tokenizer used in GPT etc.
	it breaks down a word into smaller words or subowords.
	
	*How it works:*
	a) starts with the character level vocabulary e.g (e.g., `"t"`, `"h"`, `"e"`, etc.).
	b) finds most frequent  pair of tokens (e.g., `"t"+"h"` → `"th"`).
	c)  Merge them into a new token.
	d) Repeat until you reach the desired vocabulary size.
	
	*example:*
		Suppose the word `"lower"` appears often.  
			start: `l`, `o`, `w`, `e`, `r`  
			Merge `"l"+"o"` → `"lo"`  
			Merge `"lo"+"w"` → `"low"`  
			Eventually, `"low"+"er"` → `"lower"`  
	    Rare words like `"lowered"` might be split as `"lower" + "ed"`.
	
	*benefits:*  
	Efficient vocabulary size + ability to handle unseen words (they get split into known subwords).  

	**Note:**  
	if we are using this tokenizer and we don't have the word in our vocab we assign `<unk>` token to it.

---

2. **WordPiece:**  
it is a tokenizer used by BERT etc.  
it is  similar to BPE but is has different merging strategy.  

*How it works*  
1. Start with a **character-level vocabulary**.  
2. Instead of merging the most frequent pairs (like BPE), **WordPiece chooses the merge that maximizes the likelihood of the training data**.  
	- i.e., it tries to balance **frequency** and **usefulness**.  
3. Builds the vocabulary step by step until the desired size is reached.  

*example*  
Suppose we want to tokenize `"unhappiness"`.  
- Initial split: `["u", "n", "h", "a", "p", "p", "i", "n", "e", "s", "s"]`  
- WordPiece vocabulary might have:  
    - `"un"`, `"##happiness"`, or  
    - `"un"`, `"##happy"`, `"##ness"`  

👉 Notice the `"##"` convention:  
- `"##something"` means it’s a **continuation subword** (not the start of a word).  
- `"un"` has no `##` because it starts a word.  
So `"unhappiness"` → `["un", "##happy", "##ness"]`.  
### **Why it matters**
- Handles rare/unknown words well.  
- Keeps vocabulary size manageable.  
- Very effective for morphologically rich languages.  
---

### 🔹 **Difference vs. BPE**
- **BPE**: merges the most **frequent pairs of characters/subwords**.  
- **WordPiece**: chooses merges that give the **highest probability of explaining the training text** (more probabilistic).  

---

