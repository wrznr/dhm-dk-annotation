layout: true
  
<div class="my-header"></div>

<div class="my-footer">
  <table>
    <tr>
      <td style="text-align:right">Sächsische Landesbibliothek – Staats- und Universitätsbibliothek</td>
      <td>Date</td>
      <td style="text-align:right"><a href="https://www.slub-dresden.de/">www.slub-dresden.de</a></td>
    </tr>
    <tr>
      <td style="text-align:right">Referat 4.3</td>
      <td />
    </tr>
  </table>
</div>

<div class="my-title-footer">
  <table>
    <tr>
      <td style="text-align:left"><b>Kay-Michael Würzner</b></td>
    </tr>
    <tr>
      <td style="text-align:left">Referat 4.3</td>
    </tr>
    <tr>
      <td style="font-size:8pt"><b>22nd May 2025</b></td>
    </tr>
    <tr>
      <td style="font-size:8pt">Seminar Datenkompetenz</td>
    </tr>
  </table>
</div>

---

class: title-slide
count: false

# Linguistic Annotation
## Techniques and tools for automatic corpus annotation

---

class: part-slide
count: false

# Text corpora

---

# Text corpora

- Collections of texts
- *linguistic* reference corpora
    + *representative* with respect to a language as a whole (or a certain historical form)
        * English: British National Corpus (Burnard, 1995)
        * German: Deutsches Textarchiv (Geyken and Klein, 2009)
- *special* corpora
    + *representative* sample of a language with respect to
        * medium: newspapers, (movie) subtitles, internet blogs, ...
        * content: infant, youth speech, ...
        * research: Penn Treebank (Marcus et al., 1993), childLex (Schroeder et al., 2014)

---

# Corpora and lexicography

- Guidance and knowledge for the lexicographer
- Means of evidence
    + Importance (“Is word *x* frequent enough for my dictionary?”)
    + Grammar (“Does word *x* have a plural?”)
    + Semantics (“Does word *x* have multiple meanings?”)
    + Pragmatics (“Is word *x* a terminus technicus?”)
- Challenge: Corpora are huge! How do we find interesting word occurrences?
- Approach: Abstraction through grouping of tokens

---

class: part-slide
count: false

# Linguistic annotation

---

# Linguistic annotation

- Historical: Manual reading and evaluation of text sources
- Not feasible for modern-size corpora: Automatic analysis tool chain
- Annotation of linguistic features as a preprocessing step
    + Easier to search through
    + Easier to identify relevant corpus records
    + Easier to perform quantitative analyses
- With respect to different levels of representation:
    + *Word*: Syllable structure, morphological segmentation, lexical semantics
    + *Word group*: Multi-word expressions, collocations, named entities
    + *Phrases*: Syntactical category and function
    + *Sentence*: Syntactical structure, semantics, textual function

---

# Linguistic annotation

- A sample tool chain:
    + Segmentation of running text into “words” and sentences: **Tokenization**
    + Assignment of base forms to (inflected) words: **Stemming**
    + Assignment of (syntactic) categories to words: **PoS tagging**
    + Determination of the dependencies between words in the sentential context: **Dependency parsing**
    + Evaluation of co-occurrences of words and phrases: **Distributional semantics**
- Can be done automatically (with reliable quality)
- Two fundamental principles of modelling linguistic knowledge:
    + Based on **manually** designed rules (i.e. *rule-based* approaches)
    + Based on **automatically** induced rules (i.e. *machine-learning* approaches)

---

# Linguistic annotation – rule-based approaches

- General approach of the so-called *artificial intelligence* up to the information age
    + Touch of unscientific nature in statistical methods
    + Especially in linguistics:
    > It's true there's been a lot of work on trying to apply statistical models to various linguistic problems. I think there have been some successes, but a lot of failures. There is a notion of success ... which I think is novel in the history of science. It interprets success as approximating unanalyzed data. ([Chomsky 2011](https://www.youtube.com/watch?v=jgbCXBJTpjs))
- Aim to understand cognitive processes and the development of an overall construct of human language processing interacting across modules

---

# Linguistic annotation – statistical approaches

- General approach to AI nowadays
    + Data-driven methods
    + Aim to optimize pattern recognition and prediction using vast data
    + Development of integrated end-to-end language processing systems
    > "In many cases, the models do not require prior linguistic knowledge but can still achieve high performance in tasks like translation, speech recognition, and text generation." ([Jurafsky & Martin, 2020](https://web.stanford.edu/~jurafsky/slp3/))
- No deeper understanding of the underlying mechanisms necessary

---

class: part-slide
count: false

# Tokenization

---

# Tokenization

- Segmentation of running text into **tokens** and **sentences**
- (Pre-)classification of tokens to support subsequent processing steps
    + Abbreviations
    + Numbers
    + Special characters
    + Foreign alphabets
    + Normalization of hyphenation
- *Rule-based*: White-space segmentation, **regular expressions** (e.g. Greffenstette, 1999)
- *Statistical*: **supervised learning** (given manually tokenized texts, cf. Jurish and Würzner, 2013)

---

# Tokenization

Critical examples
```
Cliff said, “I am pleased to support ACET. ...”
„Harry Potter“-Roman
Es waren mehr als 800.
¯\_(ツ)_/¯
http://www.google.co.uk/search?q=url&ie=utf-8&oe=utf-8&aq=t&rls=org.mozilla:en-GB:official&client=firefox-a
```

---

# Tokenization

- **Challenges**:
    + Ambiguous characters (especially `FULL STOP`)
    + Tokens containing white-space (e.g. complex compounds)
    + Non-terminated sentences (e.g. headings)
    + Non-standard texts (e.g. chats, tweets)
- Extremely complex rule-based systems
    + Several hundreds of interacting rules
    + Hard to maintain and adjust
- Manual tokenization is easy!
    + Training materials available
    + Low-cost adaption to specific genres

---

# Tokenization

- Sketching a machine-learning approach
    + Define a sequence representation:
        * E.g. running text as sequence of characters
    + Define a classification scheme:
        * E.g. each character can start a token (binary classification)
    + Create some training data:
        * `(A,1),(␣,1),(t,1),(e,0),(s,0),(t,0),(.,1)`
    + Select a model family:
        * Hidden Markov Model, Conditional Random Field, Neural Network, ...
    + Start training
- Test at [www.dwds.de/waste](http://kaskade.dwds.de/waste/demo.perl)

---

class: part-slide
count: false

# Morphological analysis

---

# Morphological analysis

- Task description:
    + Assignment of **possible** word categories
      ```
      greens ↦ {verb, noun}
      Carpenter  ↦ {noun, proper name}
      ```
    + Assignment of a canonical **base form** (*stem* or *lemma*)
      ```
      greens ↦ green
      Carpenter's  ↦ Carpenter
      ```
    + Identification of word formation processes
      ```
      basketball ↦ basket<NN>#ball<NN>
      happily ↦ happy<ADJ>~ly<ADV>
      ```

---

# Morphological analysis

- Machine learning not suitable/successful
    + Manual segmentation is difficult!
- Challenging for languages with complex word formation (e.g. Finnish, German, Turkish)
- `Finite State Morphology` (rule-based approach):
    + Recipe:
        * Take a **huge** list of simple (monomorphematic) words
        * Encode their morphological features
        * Add pre- and suffixes
    + Put everything into a finite-state automaton
    + Apply its **Kleene closure**
- Reduced to [**stemming**](https://text-processing.com/demo/stem/) (i.e. the removal of pre- and suffixes) for English

---

# Morphological analysis

- Illustration
    + Lexicon `{schön<A>,Geist<N>}`
    + Prefixes `{un<p>,ur<p>}`
    + Suffixes `{heit<N>,lich<A>}`

.center[.img-orig[![Lexicon](img/morph_ex.svg)]]

---

count: false

# Morphological analysis

- Illustration
    + Lexicon `{schön<A>,Geist<N>}`
    + Prefixes `{un<p>,ur<p>}`
    + Suffixes `{heit<N>,lich<A>}`

.center[.img-orig[![Lexicon](img/morph_ex2.svg)]]

---

count: false

# Morphological analysis

- Illustration
    + Lexicon `{schön<A>,Geist<N>}`
    + Prefixes `{un<p>,ur<p>}`
    + Suffixes `{heit<N>,lich<A>}`

<center><img src="img/morph_ex3.svg" width="650" /></center>

---

count: false

# Morphological analysis

- Illustration
    + Lexicon `{schön<A>,Geist<N>}`
    + Prefixes `{un<p>,ur<p>}`
    + Suffixes `{heit<N>,lich<A>}`

<center><img src="img/morph_ex4.svg" width="880" /></center>

---

count: false

# Morphological analysis

- Illustration
    + Lexicon `{schön<A>,Geist<N>}`
    + Prefixes `{un<p>,ur<p>}`
    + Suffixes `{heit<N>,lich<A>}`

<center><img src="img/morph_ex6.svg" width="880" /></center>

---

count: false

# Morphological analysis

- Illustration
    + Lexicon `{schön<A>,Geist<N>}`
    + Prefixes `{un<p>,ur<p>}`
    + Suffixes `{heit<N>,lich<A>}`

<center><img src="img/morph_ex5.svg" width="880" /></center>

---

# Morphological analysis

- Statistical approaches exist!
    + **supervised**:
        * Sequence classification (e.g. Würzner and Jurish, 2015)
    + **unsupervised**:
        * Based on character distribution (e.g. Creutz and Lagus, 2007)
- Results are not satisfying!
- Subject to many scientific competitions
    + Potential for new ideas

---

class: part-slide
count: false

# PoS tagging

---

# PoS tagging

- Selection of the **most probable** word class in the sentential context from the set of **possible** word classes
- Machine-learning approach, trained using **manually categorized** data, e.g.:
    + Hidden Markov Model using trigrams of words (and class sets, cf. Jurish, 2003)
        * Computation of the most probable sequence of word classes using the Viterbi algorithm
    + (Heuristic selection of the simplest **morphological analysis** for the determined PoS)
    + Easily adaptable to specific text genres (historical language, child language etc.)
- Best-known tool [`TreeTagger`](https://copa-trad.ufsc.br/#tree-tagger-cloud) (multilingual)

---

# PoS tagging

.center[<img src="img/tagging_with_moot.png" style="width:800px"/>]

Test the [DWDS analysis tool chain](http://kaskade.dwds.de/demo/cab/)

---

# PoS tagging

- Again, manual PoS tagging is easier than modelling the principles of word class distribution!
    + Machine learning approach instead of rule system
- Important for languages with word class ambiguity (e.g. English, German)
- Subject of many scientific competitions
- Maybe the most important annotation step for lexicographic work

---

# PoS tagging

.center[<img src="img/kohl1.svg" style="width:800px"/>]

---

count: false

# PoS tagging

.center[<img src="img/kohl.svg" style="width:800px"/>]

---

class: part-slide
count: false

# Dependency Parsing

---

# Dependency Parsing

- Analysis of the **structural dependencies** between words within a sentence
- Not to be confused with constituency parsing:

---

count: false

# Dependency Parsing

- Analysis of the **structural dependencies** between words within a sentence
- Not to be confused with constituency parsing:

.center[<img src="img/dependency_ex.svg" style="width:800px"/>]

---

# Dependency Parsing

- All words in relation to the **root** of the sentence (usually the finite verb)

.center[<img src="img/dependency_ex2.png" style="width:400px"/>]

- Often implemented using **grammar rules**
    + No straightforward representation as sequence classification
    + Small number of atomics (word classes)
- Neural networks are taking over!

---

# Dependency Parsing

- Many available tools
    + `Stanford Parser`
    + `ParZu`
    + [`spaCy`](https://explosion.ai/demos/displacy)
- Important for lexicographic work
    + Usage of words
- Used in combination with collocation extraction (see below)
    + [DWDS word profile](https://www.dwds.de/wp?q=)

---


class: part-slide
count: false

# Named Entity Recognition

---

# Named Entity Recognition

- Part of linguistic annotation at the **word group** level
- Goal: automatic detection of **named entities** such as:
    + Persons (`PER`)
    + Locations (`LOC`, `GPE`)
    + Organizations (`ORG`)
    + Dates (`DATE`), events (`EVENT`) etc.
- Common use cases:
    + **Text mining**, biography extraction
    + **Digital history**, historical editions
    + **Search systems**, **knowledge graphs** (e.g. Wikidata)

> “Goethe visited Weimar in 1775.”  
> → `PER`: *Goethe*, `LOC`: *Weimar*, `DATE`: *1775*

---

class: part-slide
count: false

# Collocations

---

# Collocations

- Expressions of **multiple words** which commonly co-occur
- *Typical* contexts of words
    + Standard feature in monolingual dictionaries since the 1940's 
    + Important for natural-sounding language (Palmer, 1933)
- Identification of *useful* corpus records for lexicography 
- Different notions of **commonly**:
    + Number of co-occurrences does not work very well
    + High-frequent (function) words, co-occurring with many words
    + Some normalization necessary

---

# Collocations

- Common measures include:
    + *Mutual Information* (MI, Church and Hanks, 1990)
    + *Student's t-test* (Manning and Schütze, 1999)
    + *Log Dice* (Rychlý, 2008)
- All measures consider co-occurrence and **non**-co-occurrence, e.g. MI:

$$
MI(x,y)=\log(\frac{P(x,y)}{P(x)P(y)})
$$

---

class: part-slide
count: false

# Many thanks for your attention!

<center>
<a href="https://wrznr.github.io/slide-template/">wrznr.github.io/slide-template</a>
</center>
