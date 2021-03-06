# Deep Contextualized Word Representations - ELMO 
## Review and Keypoints
- This paper introduces a new type of word representations which models context and captures
    - Complex characteristics of word use (Syntax and semantics)
    - How these uses vary across linguistic contexts.
- The word vectors are learned functions of the internal states of a deep biLM, which is pretrained on a large text corpus.
    - Each token is assigned a representation that is a function of the entire input sentence. 
    - ELMO(embeddings from language models) representations are deep because they are a function of the internal layers of the biLM.
    - The higher level LSTM states capture context-dependent aspects of the word and the lower level LSTM states model aspects of syntax. 
- Deep representations outperform those derived from the top layer of the LSTM.
- ELMO embeddings are functions of entire input sentences ( Thus concluding that elmo gives out sentence representations).
- The final model uses L=2 biLSTM layered with 4096 units and 512 dimension projections and a residual connection from the first to second layer. The context insensitive type representation uses 2048 character n-gram conv filters, followed by 2 highway layers projecting down a 512 vec representation. The biLM provides three layers of representations for each input token. 
- It can be used in a pipeline wherein, the pretrained elmo gives representations to the downstream task and is further finetuned on domain specific data which could finetune the representations for the downstream task. (Has been shown to improve performance for QA, SNLI etc.)
<img src='../Images/ELMO.jpg'>
- ELMO is usually included at both the input and the output layer for SNLI and SQuad, while for SRL only the input layer was better. 

## Understanding how it is trained & the math
- The way bidirectional models work is given a sequence of tokens (t<sub>1</sub>,t<sub>2</sub>,..,t<sub>N</sub>), the probability for token k, t<sub>k</sub>, is calculated as follows:
    - First the forward model, Like a general forward LM, t<sub>1</sub>.. t<sub>k-1</sub>, are used to predict the t<sub>k</sub> word. 
    - The reverse model of a LM, is similar to the forward model but the sequence in reverse is used to predict the t<sub>k</sub> word.
    - A bidirectional language model combines both of them to give the following formulation which is maximized by log likelihood:
        <img src='../Images/ELMO1.jpg'>
    - The parameters of the token representation and the softmax layer are tied.
    - In a sense, the modeling here gives the biLM a sense of the context since it sees the entire sequence in forward and reverse before predicting the word. 
    - Where does the softmax layer come? The LM in both cases is first used to compute a context independent token embeddings and is put through L layers of LSTMs, and the final representation is put through a softmax layer to compute the predict next token.
- ELMO formulation
    - For each token k, a L-layer biLM computes a set of 2L+1 representations. (2L because each LM in the forward and reverse direction has L layers and the +1 from the context independent representation that is computed).
    - For downstream tasks, all layer representations are reduced to a single vector. Therefore at the end the ELMO would produce 3 representations.