## **The MALACH Corpus: Results with End-to-End Architectures and Pretraining**

* **Task**: Lower the WER on test data utilizing various boosting techniques like N-gram language model, beam search, and rescoring with LLMs(e.g., BERT, GPT2)

* **Test Datasets**: 
    * Original: /scratch/qy692/capstone/datasets/new_data/new_test_tsv
    * Segmented: /scratch/qy692/capstone/Segmentation/reseg_new_test_tsv

* **Acoustic Model**: wav2vec fine-tuned on all original data, /vast/map22/malachtest/all-e3-w500-freeze-newparms-bs10 

* **N-gram Language Model**: /scratch/qy692/capstone/4-gram.arpa (trained on Librispeech)

* **Beam Search + N-gram Implementation**: [pyctcdecode](https://github.com/kensho-technologies/pyctcdecode) used, a fast and feature-rich CTC beam search decoder for speech recognition written in Python, providing n-gram (kenlm) language model support similar to PaddlePaddle's decoder, but incorporating many new features such as byte pair encoding and real-time decoding to support models like Nvidia's Conformer-CTC or Facebook's Wav2Vec2.

* **Causal Language Model**: GPT2 fine-tuned on all original data, /scratch/qy692/capstone/test-clm, followed the instructions [here](https://github.com/huggingface/transformers/tree/main/examples/pytorch/language-modeling).

### **Experiment Results**

| Search Method| Test Set | WER|
|- |- |- |
|Greedy Search| Original| 12.8%| 
|Greedy Search| Segmented|11.9%| 
|Beam Search + 4-gram| Original| 12.3%| 
|Beam Search + 4-gram| Segmented|11.5%| 
|0.5 * Beam Search + 0.4 * 4-gram + 0.2 * GPT2| Original|11.9%| 
|0.5 * Beam Search + 0.4 * 4-gram + 0.2 * GPT2 | Segmented|11.1%| 
|0.5 * Beam Search + 0.4 * 4-gram + 0.0 * GPT2| Original|12%| 
|0.5 * Beam Search + 0.4 * 4-gram + 0.0 * GPT2 | Segmented|11.2%| 
|0.5 * Beam Search + 0.4 * 4-gram + 0.1 * GPT2| Original|11.1%|
|0.5 * Beam Search + 0.4 * 4-gram + 0.1 * GPT2 | Segmented | 11.1% 
|0.5 * Beam Search + 0.4 * 4-gram + 0.3 * GPT2 | Segmented |11.1% | 
|0.5 * Beam Search + 0.4 * 4-gram + 0.4 * GPT2 | Segmented |11.1% | 
|0.5 * Beam Search + 0.5 * 4-gram + 0.5 * GPT2 | Segmented |11.1% | 
|0.0 * Beam Search + 0.5 * 4-gram + 0.5 * GPT2 | Segmented |11.3% | 
|0.3 * Beam Search + 0.3 * 4-gram + 0.5 * GPT2 | Segmented |11.1% | 
|0.0 * Beam Search + 0.5 * 4-gram + 0.5 * GPT2 | Segmented |11.3% | 
|Greedy Search| Segmented & Cleaned |11.1%| 
|Beam Search + 4-gram| Segmented & Cleaned |10.7%| 
|0.5 * Beam Search + 0.5 * 4-gram + 0.5 * GPT2 | Segmented & Cleaned |10.3%| 

Note:

* All the experiments were done on the test datasets where corrupted records were removed (Speaker 00017 removed).
* 4-gram actually stands for `beam search + 4-gram`, the weights for beam search and 4-gram are 0.5 and 0.5 respectively when calculating the general score.