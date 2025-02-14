# Document Similarity Using Hadoop MapReduce

## 📌 Objective
The goal of this assignment is to compute the **Jaccard Similarity** between pairs of documents using **MapReduce** in Hadoop. This project will:
- Extract words from multiple text documents.
- Identify common words appearing in multiple documents.
- Compute the **Jaccard Similarity** between document pairs.
- Output document pairs with a similarity score above **50%**.

---

## 📥 Example Input
The input consists of multiple text documents stored in HDFS. Each document contains several words, and the task is to compare all pairs of documents.

### Example Documents
#### `doc1.txt`
```
hadoop is a distributed system
```
#### `doc2.txt`
```
hadoop is used for big data processing
```
#### `doc3.txt`
```
big data is important for analysis
```

---

## 📏 Jaccard Similarity Calculator

### Overview
**Jaccard Similarity** measures the similarity between two sets and is defined as the **size of the intersection** divided by the **size of the union** of two sets.

### Formula
\[
Jaccard Similarity = \frac{|A \cap B|}{|A \cup B|}
\]
Where:
- **|A ∩ B|** = Number of words common in both documents.
- **|A ∪ B|** = Total number of unique words in both documents.

### Example Calculation
Comparing `doc1.txt` and `doc2.txt`:
- **Words in `doc1.txt`**: `{hadoop, is, a, distributed, system}`
- **Words in `doc2.txt`**: `{hadoop, is, used, for, big, data, processing}`
- **Common words**: `{hadoop, is}` (2 words)
- **Total unique words**: `{hadoop, is, a, distributed, system, used, for, big, data, processing}` (10 words)
- **Jaccard Similarity**: \( \frac{2}{10} = 0.2 \) (20%)

---

## 🚀 Implementation

### 🗂 Approach
1. **Mapper**: Reads each document and emits `(word, document_id)` pairs.
2. **Reducer**: Groups words and their document occurrences, forming document sets.
3. **Final Processing**: Computes the **Jaccard Similarity** for all document pairs and outputs those with **similarity > 50%**.

### 🔧 Implementation Details
- **Mapper**: Extracts words from documents and associates them with document IDs.
- **Reducer**: Creates word sets for each document and computes similarity.
- **Driver Class**: Orchestrates the MapReduce job execution.

---

## 📦 Environment Setup: Running Hadoop in Docker

### Step 1: Install Docker & Docker Compose
- **Windows**: Install [Docker Desktop](https://docs.docker.com/desktop/install/) and enable **WSL 2 backend**.
- **macOS/Linux**: Install Docker using the official [installation guide](https://docs.docker.com/engine/install/).

### Step 2: Start the Hadoop Cluster
Navigate to the project directory and run:
```bash
docker-compose up -d
```
This starts Hadoop NameNode, DataNode, and ResourceManager.

### Step 3: Access the Hadoop Container
```bash
docker exec -it hadoop-master /bin/bash
```

---

## 📦 Building and Running the MapReduce Job with Maven

### Step 1: Build the JAR File
Ensure **Maven** is installed, then compile the project:
```bash
mvn clean package
```
This generates a JAR file inside the `target` directory.

### Step 2: Copy the JAR File to the Hadoop Container
```bash
docker cp target/similarity.jar hadoop-master:/opt/hadoop-3.2.1/share/hadoop/mapreduce/similarity.jar
```

---

## 📂 Uploading Data to HDFS

### Step 1: Create an Input Directory in HDFS
Inside the Hadoop container, create the directory:
```bash
hdfs dfs -mkdir -p /input
```

### Step 2: Upload Dataset to HDFS
```bash
hdfs dfs -put /path/to/local/input/* /input/
```

---

## 🚀 Running the MapReduce Job
Run the Hadoop job inside the container:
```bash
hadoop jar similarity.jar DocumentSimilarityDriver /input /output_similarity /output_final
```

---

## 📊 Retrieving the Output
### View Results in HDFS
```bash
hdfs dfs -cat /output_final/part-r-00000
```

### Download Output to Local Machine
```bash
hdfs dfs -get /output_final /path/to/local/output
```

---

## 💡 Challenges & Solutions

### 1️⃣ **Handling Large Data Files**
**Challenge**: Some documents were too large for a single mapper.
**Solution**: Used Hadoop's built-in **TextInputFormat**, which automatically splits large files into smaller chunks.

### 2️⃣ **Handling Stop Words**
**Challenge**: Common words (e.g., *is, a, for*) were inflating similarity scores.
**Solution**: Implemented a **stop-word filtering mechanism** in the Mapper.

### 3️⃣ **Memory Management for Large Pairs**
**Challenge**: Large document collections resulted in excessive key-value pairs.
**Solution**: Used a **combiner** to reduce intermediate data before sending it to the Reducer.

---

## 📢 Conclusion
This project successfully demonstrates how **Hadoop MapReduce** can be used to compute document similarity efficiently. By leveraging the **Jaccard Similarity** metric, we extracted meaningful relationships between documents, a fundamental technique in text analytics, recommendation systems, and plagiarism detection.

### ✅ Key Takeaways:
- **MapReduce** is highly effective for large-scale text processing.
- **Jaccard Similarity** provides a simple yet powerful metric for comparing documents.
- **Proper data preprocessing** (e.g., stop-word removal) significantly improves accuracy.

---

## 🔗 References
- [Apache Hadoop Documentation](https://hadoop.apache.org/docs/stable/)
- [Docker & Hadoop Setup Guide](https://towardsdatascience.com/running-hadoop-on-docker-a-step-by-step-guide-84b39f2aa4f4)

🚀 **Happy Coding!**

