NLP is a field of AI that enables computers to **understand**, **interpret** and **generate** human language

We have **two levels** of representation

- **Document-level**
	- Representation level: **Direct document vectors**
	- ![[Pasted image 20260225124214.png]]
- **Word-level** (Word2Vec)
	- Representation level: **Word vectors** - can **aggregate** to form **document vectors**
	- ![[Pasted image 20260225124227.png]]

---

First, we have **text pre-processing pipelines**
- Raw data from natural language is **unstructured** and **noisy**
- Text pre-processing **transforms text** into a **clean** and more **structured** format for further analysis and learning

The pipeline:
- **Sentence segmentation**
	- The process of **breaking text** into **sentences** (often using punctuation marks)
	- Also known as **sentence boundary disambiguation/detection**
	- **First step** in NLP pre-processing
	- ![[Pasted image 20260225124800.png]]
	- Challenges:
		- Full stop do **not always mean** the **end** of a sentence
		- It can denote an abbreviation, decimal point or an email address
		
- **Word Tokenisation**
	- A **token** is an element or a **unit** of semantics that carries **meaning**
		- Tokens can be words, numbers, symbols, punctuation marks
	- **Word tokenisation** is the process of splitting sentences into **words (tokens)**. It is the **key step** in NLP
	- ![[Pasted image 20260225125103.png]]
- **Stop-words removal**
	- 'Stop words' Are the **most common** words that **do not** carry **important meaning** from the text
		- **Articles** and **pronouns** are generally stop words
	- We remove them in order to **focus** on **important information** from text
		- **Reduces** the dataset **size**, thus saving training time
	- Some NLP libraries have **default stopword lists** including all **auxiliary verbs**, **pronouns**, and **negations**
		- However, in **sentiment** analysis we cannot remove stopwords **blindly**
		- ![[Pasted image 20260225125255.png]]
- **Stemming**
	- **Reduces** words to their **root** or **base forms**
		- Often done by **removing** **prefix** or **suffix** of words, which may result in an invalid dictionary word
		- ![[Pasted image 20260225125342.png]]
	- **Reduces** vocabulary **size**
	- **Groups** words with **similar meanings**
	
- **Lemmatisation**
	- **Normalisation** technique that **reduces** words to their **dictionary forms**, called *lemma*
		- ![[Pasted image 20260225125450.png]]
	- Stemming is based on simple heuristic rules and runs faster
		- It often results in roots that may not be valid words
		- Unlike stemming, lemmatisation always produces a **valid word** which relies on a dictionary to look up matching words
- **PoS Tagging**
	- PoS (Part-of-Speech) tagging is the process of **labelling** each word in a text with its **grammatical cetegory** (noun, verb, adjective)
		- ![[Pasted image 20260225125615.png]]
	- Some words may have **multiple** PoS tags
		- PoS tagging uses **context** to decide the correct tag.
		- ![[Pasted image 20260225125642.png]]
	- Used for different use-cases:
		- ![[Pasted image 20260225125657.png]]
---

A **corpus** is a collection of text documents

A **vocabulary** is a collection of all **unique** words in the corpus

We explore some **document-level** representation techniques 

---

**Bag of Words** (BoW)

The Bag of Words represents each document as a vector, where each $i$-th entry records the **number of times each word $i$ appears** in a document
- It is called a '*bag*' of words because it puts words in a bag and counting them while **discarding** their **order** and **semantics**
- It is viewed as a **histogram** of **word frequencies** in a document

It fills out a **document-term** matrix, where each row is a feature vector of length of the number of unique words.

Pros:
- BOW is simple and **easy to implement**
- Runs very **fast** and is **computationally efficient**
Cons:
- Document-term matrix is **often** very **sparse**
	- To reduce this we could use, **Sparse** **Storage**, and LSA algorithm
- BOW only counts word **frequency**, but **ignores** how **frequent** each word is across **all documents**
- BOW neglects **word order** in the context
- BOW cannot **quantify similarity** between words that are **synonyms**

**Sparse Storage** format

Yale (or CSR, *compressed sparse row*) format represents a matrix by **three arrays** that respectivley contain:
- `Offset of row`
- `Column index`
- Nonzero value

This format allows **fast row access** and matrix-vector multiplications

**Coordinate List** format

COO format stores a list of **tuples** in the form of 
- `row index`
- `column index`
- `nonzero value`

Ideally, the entries are sorted first by row index and then by column index, to improve random access times
- This format is good for **incrememental** matrix construction

**TF x IDF**
- Term Frequency-Inverse Document Frequency

A scoring approach to identify words that are frequent in a document but rare in the corpus.

TF:
- Measures how frequently a word $w$ occurs in a document $d$ **relative** to the document **length**

TF of a term $w$ in document $d$ is:
$$\dfrac{\text{num times word appears in document d}}{\text{num words in document d}}$$

However, some words may appear **frequently** in **every document** (e.g. 'data')
- They are not useful to distinguish documents - TF cannot detect this

**IDF** measures how **unique** or **rare** a word is across all documents
- Defined by:

$$log(\dfrac{\text{num documents in corpus $D$}}{\text{num documents that contain word $w$}})$$

Rare words - More informative - High IDF
Frequent words - Less informative - Low IDF
- The log function prevents rare words from receiving too large weights while still rewarding rarity

We multiply TF by IDF to get an overall score representing the importance of words in the document in relation to the count of the word in all documents.

---

**Bag of** $N-Grams$

Avoids the context-loss problem

An N-Gram is a **sequence** of $N$ **contiguous words** from text
- May be **more understandable** than a single word since they capture **local word order** and **more context** around each word (e.g. 'New York City' vs 'New', 'York', and 'City')

Like BOW, Bag of N-grams **counts** the number of times that each N-gram appears in each document.
- However, bag of N-grams may produce a **larger**, **sparser** feature set than BOW

---

LSA
- A **latent semantic analysis** (LSA) model uses the **singular value decomposition** (SVD) of a document-by-word matrix to represent the relationships between documents and the words.
- An LSA model is a **dimensionality reduction** tool useful for running low-dimensional models on high-dimensional word counts

**Assumptions** of LSA
- The words used in the **similar context** are similar to each other
- The hidden semantic structure is **unclear** due to **word ambiguity**

SVD:
- Given a document-by-word matrix $A$, and a **low rank** $r$ (user-specified)
- Find: a **low-rank** SVD of $A$ such that:
$$A\approx U\cdot\Sigma\cdot V^{T}$$

This can find the **latent** (hidden) **semantic structure** of words spread across documents

---

Next, we go through **word-level** representations

**Word2Vec**

Identifies **dense vector representation** for each individual word in the **corpus**

Word embedding is a dense, low-dimensional vector representation of words that capture **semantic meaning** and **contextual similarities**
- Word2Vec is a **shallow neural network-based** method for learning word embeddings 

Key intuition:
- Words appearing in **similar contexts** have **similar meanings**
- Distributional hypothesis
	- '*you shall know a word by the company it keeps*'

The traditional encoding scheme (before word2vec), was called **one-hot encoding**
- Each word - $|V|$-dimensional unit vector 
	- Where $|V|$ = the vocabulary size
- Two **distinct** one-hot vectors are **orthogonal** to each other

As a result, one-hot encoding is:
- Very high dimensional ($|V|\approx 60,000$ )
- Sparse (only 1 nonzero in $e_{i}$)
- No semantic meaning ($e_{cat}^{T}\cdot e_{kitten} = 0 \rightarrow \text{cat} \bot \text{kitten}$) 

However **word embeddings**:
- are **low dimensional**
- **dense**
- capture **similarity**
- Enable **analogical reasoning**
	- $v_{cat}-v_{kitten}\approx v_{horse}-v_{pony}$

So how do we get these word embeddings / dense vector representations?

A couple ways:
- **Continuous bag of words** (CBOW)
	- *Given* surrounding **context words**
	- *Predict* a **target word**
- **Skip-gram**
	- *Given* a **target word**
	- *Predict* surrounding **context words**

How to obtain the training pairs? (context words, target word)

In Word2Vec, the **window size** determines how many surrounding words are used as context words
- Window size = $k$: We use $k$ words on the left, and $k$ words on the right.
- So total number of context words = $2k$

In practice, a **small** window (2-3) captures **syntactic relationships** and **grammar patterns**
- Alternatively, a **large** window (5-10) captures **semantic similarity** and **topic-level representations**
- The window size could also be set as a dynamic parameter (to learn) rather than a hyper-parameter

After setting the window size, we **slide** the **target word** across the sentence to generate training samples

**CBOW network**
- After generating training samples through a sliding window, we train a network based on CBOW to **learn dense vector representations** for all words in the corpus vocabulary

After training, each column of $W$ is a **word vector** (embedding)

CBOW sums context words to predict the target word
- Faster
- Larger dataset size
- Weaker performance on rare words
Skip-gram uses the target word to predict each context word individually
- Slower
- Small dataset size
- Better performance on rare words