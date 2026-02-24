# DoS-Classifier

### Application Area

Our application area is in network classification. We hope to develop a classifier that can distinguish between benign traffic and DOS attacks.

### Dataset Selection

The SUEE1 and dataset is a PCAP file recording about 2 million packets containing both benign traffic and instances of several DOS attacks. Limitations of the dataset include the fact that it is several years old and contains only a few distinct attack implementations (which may not be representative of the range of modern attacks) and data imbalance (about 150 attackers are present out of several thousand hosts).

The dataset can be found here: https://github.com/vs-uulm/2017-SUEE-data-set?tab=readme-ov-file

This page also hosts the SUEE8 dataset, which is larger but follows the same format. We may use this instead, or a combination, if this is computationally viable.

### Data Type Characterization

The PCAP files appear to be in a structured format, organized per-packet.

The data are labeled as specific ranges of IP addresses were used for running the attacks. (This does mean that we cannot meaningfully train on IP addresses as a predictor variable). 

Data is largely quantitative, although may contain some readable qualitative data.

Data is time-dependent.

### Data Science / Machine Learning Problem Definition

Our objective is to classify traffic as benign or adversarial (DOS). Since attacks were run from a specific range of IP addresses, these can be used to generate output labels. Any other features that can be extracted from the PCAP network captures, including sender, timestamp, length, other metadata, can be used 

### Why Data Science / Machine Learning?

ML approaches are well-suited to produce complex or irregularly-structured rulesets for detecting attacks, which makes it more difficult for attackers to predict what attack behavior will  remain undetected. (If a simple ruleset is used, attackers may infer the rules that are being used and tune their attack strategy to avoid violations). This unpredictability can also be a drawback, but since DOS attacks tend to require a significant volume of attacking traffic, an unreliable classifier may still be effective for alerting defenders to the issue and at least a sample of its source. This can be combined with straightforward filtering approaches, used proactively and/or reactively.

### Planned Approaches

Our first possible approach to data processing is to treat each packet as a ‘row’ and derive features from its metadata and contents, as well as, potentially, the previous packet(s) from the same sender. Much of this metadata is either quantitative or vaguely categorical, which may require some combination of scaling and encoding. The packet bodies represent unstructured data, which we may investigate using text-processing techniques (n-gram, BoW, TFIDF) although further examination will be required to determine how much of this data is available and readable. This approach will use time-series classification model(s) that attempt to label individual packets as either suspicious or benign.

Another approach is to summarize data over all traffic from a particular host (or potentially a particular host in a particular timeframe). This could include quantity and frequency statistics, average values of continuous features and frequencies of categorical feature values. We may also be able to run a clustering algorithm on packets and then use summaries of this data to make a judgement on a host.

One very important processing step is ensuring that IP addresses (used as labels) are not otherwise present in the predictor features during training.

### Evaluation Plan

Our primary metric will be classification F1 score, since this is robust to unbalanced data, although this may vary if we train multiple models with different X/Y choices. Statistics that highlight false positives vs negatives, such as precision vs recall, will also be useful to detect whether the model is overly or insufficiently suspicious. We hope to use cross-validation to estimate model performance.
