# Bottom Up Abstractive Summarization

## Summary 
- To overcome poor content selection, the authors model a data efficient content selector to over-determine phrases in a source document that should be part of the summary. This selector acts as a bottom up attention step to constrain the model to likely phrases.
- This is a two step process and not end to end, very data efficient making it easy to transfer a trained summarizer to a new domain. 
- Selection Model :
  - The selection tasks is framed as a sequence tagging problem, with the objective of identifying tokens that are part of its summary. 
  - Models which build on contextual word embeddings can identify correct tokens most of the time. 
  - Masking is employed to constrain copying words to the selected parts of the text, which produces grammatical outputs.
  - Data is generated by aligning the summaries to the document.
  - Use a standard biLSTM with MLE for seq labeling.
  - The copy attention is directly replaced by the words selected by this selector. 
- A pointer generator with copy attention is used as the abstractor. 


## Strengths
- Makes clear the importance of context and content selection while summarizing documents.
