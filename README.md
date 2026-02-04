## BERT-based Network Intrusion Detection

This notebook demonstrates how to build and train a BERT-based model for network intrusion detection using network traffic data.

### 1. Importing File
This section handles mounting Google Drive to access the dataset and loading a `.csv` file into a pandas DataFrame. The dataset used is `Thursday-WorkingHours-Morning-WebAttacks-EDITED-ONLYBRUTEFORCE.pcap_ISCX.csv`.

### 2. Data Preprocessing
This part of the notebook preprocesses the raw data:
- The ' Label' column is transformed into a binary format (0 for 'BENIGN', 1 for others).
- Protocol numbers are mapped to their corresponding names (e.g., 6 to 'TCP', 17 to 'UDP').
- A new feature called 'Flow ID Plus' is created by concatenating various flow identifiers (Destination IP, Source IP, Destination Port, Source Port, Protocol Name) into a single text string. This text string will be the input for the BERT model.

### 3. Tokenization and Loading Base Model
- The `transformers` library is installed.
- An `AutoTokenizer` and `AutoModel` are loaded from the pre-trained `bert-base-uncased` model.
- The 'Flow ID Plus' text strings are tokenized and converted into PyTorch tensors, which are suitable inputs for the BERT model.

### 4. Splitting of Dataset for Test and Train
- A custom PyTorch `TrafficDataset` class is defined to handle the tokenized inputs and labels.
- The dataset is split into training and testing sets (70% training, 30% testing).
- Class weights are calculated for the training data to address potential class imbalance, which is common in intrusion detection datasets where benign traffic is much more frequent than malicious traffic.
- `DataLoader` instances are created for both training and testing datasets to facilitate batch processing during training.

### 5. Training and Testing
- A `BertForSequenceClassification` model is loaded with two labels (binary classification: benign or malicious).
- The model is moved to a GPU if available.
- An `AdamW` optimizer is configured with a learning rate of 2e-5.
- A weighted `CrossEntropyLoss` function is used, incorporating the previously calculated class weights to give more importance to the minority class (malicious traffic).
- The model is trained for 3 epochs, with training and evaluation performed at each epoch.

### 6. Evaluation Metrics: Confusion Matrix and ROC Curve
- After training, the model's performance is evaluated on the test set.
- Predictions and probabilities are collected for all test samples.
- A **Confusion Matrix** is generated to show the counts of true positives, true negatives, false positives, and false negatives.
- A **Receiver Operating Characteristic (ROC) Curve** is plotted, and the Area Under the Curve (AUC) is calculated to assess the model's ability to distinguish between the two classes across various classification thresholds.
