# Empirical Regularities of Language

A computational analysis of Shannon entropy and Zipf's Law in natural language processing, exploring fundamental patterns in English text.

## Overview

This notebook investigates two key empirical regularities in natural language:

1. **Shannon Entropy**: Measures the predictability of letters in English text based on varying amounts of context
2. **Zipf's Law**: Analyzes the frequency distribution of words, showing that word rank multiplied by frequency approximates a constant

## Theoretical Background

### Shannon Entropy

Shannon entropy quantifies the unpredictability or information content in a sequence. For text prediction with N characters of context:

- **Upper Bound**: $F_N = -\sum_{i=1}^{27} q_i^N \log_2 q_i^N$ where $q_i^N$ is the probability of taking i guesses
- **Lower Bound**: $\sum_{i=1}^{27} i(q_i^N - q_{i+1}^N) \log_2 i$

### Zipf's Law

Zipf's Law states that a word's frequency is inversely proportional to its rank: $r \cdot f = k$

Or in terms of relative frequency: $r \cdot P_r = c$, where $P_r = f/N$ and $c = k/N$.

## Dependencies

```bash
pip install pandas matplotlib
```

Required Python libraries:
- `pandas` - Data manipulation and analysis
- `matplotlib` - Plotting and visualization
- `json` - JSON parsing
- `gzip` - Compressed file handling
- `re` - Regular expressions for tokenization
- `math` - Mathematical operations
- `collections` - defaultdict for counting

## Setup and Usage

### 1. Shannon Entropy Analysis

The notebook uses data from the [Shannon Game](https://www.ccs.neu.edu/home/dasmith/courses/cs6120/shannon/) where participants guess letters with varying context:

```python
# Guess data for three texts
text2_guesses = [17, 5, 6, 2, 1, 1, ...]
text3_guesses = [1, 5, 3, 18, 25, 3, ...]
text4_guesses = [5, 1, 26, 1, 1, 2, ...]
```

The code computes entropy bounds for each context length (0-67 characters).

### 2. Zipf's Law Analysis

Download the corpus:
```bash
wget "http://khoury.northeastern.edu/home/dasmith/pg-sample.json.gz"
```

Or run directly in the notebook - the script handles downloading automatically.

### 3. Running the Analysis

Execute the entire notebook/script to:
1. Calculate Shannon entropy bounds
2. Tokenize 1000 books from Project Gutenberg
3. Compute unigram, bigram, and trigram frequencies
4. Generate log-log plots of rank vs. relative frequency

## Key Features

### Text Processing
- **Tokenization**: Splits text on whitespace and punctuation
- **Normalization**: Converts to lowercase, filters tokens with at least one letter
- **N-gram extraction**: Computes unigrams, bigrams, and trigrams

### Statistical Analysis
- Frequency counting for all n-grams
- Relative frequency computation (frequency/total_tokens)
- Rank-based sorting

### Visualization
- Log-log scatter plots showing Zipf's Law
- Separate plots for unigrams, bigrams, and trigrams
- Grid overlays for easier reading

## Results

### Shannon Entropy
The analysis shows that entropy (unpredictability) decreases as context length increases, demonstrating that more context makes letter prediction easier.

### Zipf's Law Distribution

**Unigrams**: Follow Zipf's law closely with slight deviation at extremes
- Small curve at high-frequency end
- Flattening tail indicating many rare words

**Bigrams**: Lower relative frequencies overall
- Longer tail than unigrams
- More diverse distribution

**Trigrams**: Most diverse distribution
- Lowest relative frequencies
- Longest tail with many low-frequency combinations
- Noisiest pattern of the three

## Implementation Details

### Matrix Construction
Creates a 28×max_length matrix tracking guess counts:
- Rows: Number of guesses (0-27)
- Columns: Context length (0-67)

### Tokenization Pattern
```python
split_pattern = re.compile(r'[^a-zA-Z0-9]+')
pattern = re.compile(r'[a-z]')
```

### N-gram Computation
Uses sliding windows over token lists:
- Bigrams: `tokens[i], tokens[i+1]`
- Trigrams: `tokens[i], tokens[i+1], tokens[i+2]`