## Experiment Code for bert generate
Bert 做 文本生成 的一些實驗  

How bert perform on text generation ?    
Here is a POC try it in three different finetuning ways  
- Generate result one by one word
- Generate result one time
- Generate from LSTM

### Colab Address  
``` 
https://colab.research.google.com/drive/19wgXJPdb_282M0_puMgQ8qid0jvmJghG
```

### Detail  
Like a Seq2Seq task, input a sentence out a sentence
### ONEBYONE
#### Input  
```text
Step 1. [CLS] I go to school by bus [SEP] [MASK]
Step 2. [CLS] I go to school by bus [SEP] 我[MASK]
Step 3. [CLS] I go to school by bus [SEP] 我搭[MASK]
Step 4. [CLS] I go to school by bus [SEP] 我搭公[MASK]
Step 5. [CLS] I go to school by bus [SEP] 我搭公車[MASK]
Step 6. [CLS] I go to school by bus [SEP] 我搭公車上[MASK]
Step 7. [CLS] I go to school by bus [SEP] 我搭公車上學[MASK]
```
#### Output  
```text
Step 1.  
Step 2.  我
Step 3.  搭
Step 4.  公
Step 5.  車
Step 6.  上
Step 7.  學
Step 8.  [SEP]
```
#### Loss Calculate
```
['[CLS]', 'i', 'go', 'to', 'school', 'by', 'bus', '[SEP]', '[MASK]']
[-1, -1, -1, -1, -1, -1, -1, -1, 2769] 
```

### ONCE
#### Input  
```text
Step 1. [CLS] I go to school by bus [SEP] [MASK][MASK][MASK][MASK][MASK][MASK][MASK]...[MASK]
```
#### Output  
```text
Step 1.  我搭公車上學[SEP]-1-1-1....-1
```
#### Loss Calculate
```
['[CLS]', 'i', 'go', 'to', 'school', 'by', 'bus', '[SEP]', '[MASK]']
[-1, -1, -1, -1, -1, -1, -1, -1, 2769] 
```
```
['[CLS]', 'i', 'go', 'to', 'school', 'by', 'bus', '[SEP]', '[MASK]', '[MASK]', '[MASK]', '[MASK]', '[MASK]', '[MASK]', '[MASK]', '[MASK]'..... '[MASK]']
[ -1,   -1,   -1,   -1,   -1,   -1,   -1,   -1, 2769, 3022, 1062, 6722, 677, 2119,  102,   -1,   -1, .... ,-1] 
```

### ONCE in LSTM   
Same as ONCE 