# Graph-based Query Reformulation System
This is PyTorch implementation of the Graph-based Query Reformulation System for Descriptive Queries of Jargon Words using a Dictionary.

## 폴더 설명

### Data 폴더
1. analysis : 모델 분석을 위한 폴더
    - analysis_{model_name}.json : 모델의 예측 결과와 사용된 모델의 타입 정보 등이 저장된 파일
        * "Query" : 쿼리 입력 문장
        * "Preprocessed Query" : 쿼리 입력 문장의 색인텀 분석 결과
        * "Answer" : 정답 (표제어 단어)
        * "Graph-Search Score" : Min-max 정규화된 Graph-Search model의 score 결과
        * "Graph-based QR Score" : Min-max 정규화된 Graph-bsed QR model의 score 결과
        * "Predict" : 모델이 예측한 표제어 결과 (null인 경우 특정 임계값을 넘지 못한 경우)
        * "Method" : Graph-Search 모델과 Graph-based QR 모델 중에서 어떤 모델 타입으로 변형을 수행했는지에 대한 결과 (null인 경우 특정 임계값을 넘지 못한 경우)
        * "Match" : 모델의 정답 여부 (true: 정답 / false: 오답 / null: 특정 thresold를 넘지 못한 경우)
        * "Graph-Search Top10" : Graph-Search 모델의 score가 높은 상위 10개의 결과
        * "Graph-based QR Top10" : Graph-based QR 모델의 score가 높은 상위 10개의 결과
        
2. bert : ETRI에서 사전 학습한 한국어 KorBERT 모델의 학습 파라미터와 사전이 저장된 폴더
    - bert_config.json : 한국어 KorBERT config 파일
    - pytorch_model.bin : 사전 학습된 한국어 KorBERT 모델  
    - vocab.korean.rawtext.list : 한국어 KorBERT의 사전 파일

3. dict : 학습에 필요한 한자어사전과 node2idx를 포함하는 폴더
    - definition_pos.txt : 한자어 사전의 설명을 형태소 분석한 파일
    - definition_term.txt : 한자어 사전의 설명을 색인텀 분석한 파일
    - hanja_dict.xlsx : 학습에 필요한 한자어 사전 파일 (1로 태깅된 표제어만을 활용해서 모델 학습)
    - node2idx_term.json : 학습에 필요한 node2idx 사전 파일

4. embed : 학습된 모델의 모델 bin 파일이 저장된 폴더
    - BERT_Embed_term.bin : 한자어 사전과 한국어 KorBERT를 활용해서 모든 노드의 초기 임베딩 벡터
    - GNN_gcn_save_term_layer_1_head_1.bin : GCN 모델의 사전 학습된 모델 (layer=1, head=1)
    - GNN_gat_save_term_layer_1_head_2.bin : GAT 모델의 사전 학습된 모델 (layer=1, head=2)
    - GNN_QR_gat_save_term_GAT_SOTA_model.bin : 제안 모델의 최종 모델 (type:GAT, SOTA)
    - GNN_QR_gcn_save_term_GCN_SOTA_model.bin : 제안 모델의 최종 모델 (type:GCN)

5. query : 모델의 평가 데이터 쿼리가 저장된 폴더
    - test : 파이프라인 모델(Graph-Search 모델과 Graph-based QR 모델)의 test 데이터 폴더
        * query_answer.txt : 쿼리의 정답(표제어) 파일 
        * query_term.txt : 쿼리의 색인텀 분석 결과 파일
        * query.txt : 쿼리의 원형 파일
    - valid : 파이프라인 모델(Graph-Search 모델과 Graph-based QR 모델)의 validation 데이터 폴더
        * query_answer.txt : 쿼리의 정답(표제어) 파일
        * query_term.txt : 쿼리의 색인텀 분석 결과 파일
        * query.txt : 쿼리의 원형 파일
    - QR_test.json : Graph-based QR 모델의 test 데이터 파일 (위의 test 폴더의 데이터와 같음.)
    - QR_valid.json : Graph-based QR 모델의 validation 데이터 파일 (위의 valid 폴더의 데이터와 같음)
    - QR_train.json : Graph-based QR 모델의 train 데이터 파일 (한자어 사전의 표제어 및 표제어의 설명)

### 모델 logging 폴더 
- {model_name}.log : 모델의 log 파일이 저장된 파일 


## Installation
For training, a GPU is strongly recommended for speed. CPU is supported but training could be extremely slow.

### PyTorch
The code is based on PyTorch and **supports PyTorch 1.7.1 now** . You can find installation instructions [here](http://pytorch.org/).

### Dependencies
The code is written in Python 3.7. Its dependencies are summarized in the file ```requirements.txt```. You can install these dependencies like this:
```
pip install -r requirements.txt
```

### KorBERT Tokenization
- tokenization.py : ETRI KorBERT 에서 제공하는 tokenization 파일. 모델 학습 시 해당 파일을 pytorch-pretrained-bert 라이브러리 폴더에 존재하는 tokenization 파일과 대체하여 사용


## Usage
**Graph-search Model**
```
bash runs.sh graph-search Graph_Search_Model 0 term test 0.0 
```
```
CUDA_VISIBLE_DEVICES=0 python -W ignore -u ./code/main.py \
    --data_dir ./data/ \
    --mode term \
    --do_graph_search \
    --dataset test \
    --graph_search_threshold 0.0 \
    --version Graph_Search_Model
```

**BERT embedding**
```
bash runs.sh bert_embedding BERT_Embedding 0 term 128 16 
```
```
CUDA_VISIBLE_DEVICES=0 python -W ignore -u ./code/main.py \
    --data_dir ./data/ \
    --mode term \
    --do_preprocessing \
    --do_bert_embedding \
    --bert_dir ./data/bert/ \
    --bert_vocab ./data/bert/vocab.korean.rawtext.list \
    --bert_max_seq_length 128 \
    --bert_batch_size 16 \
    --version bert_embedding
```

**GCN Pre-training**
```
bash runs.sh pretraining_gnn Pretraining_GCN 0 term gcn 5e-5 10000 1 1 
```
```
CUDA_VISIBLE_DEVICES=0 python -W ignore -u ./code/main.py \
    --gnn_model gcn \
    --data_dir ./data/ \
    --mode term \
    --do_preprocessing \
    --do_gnn \
    --do_gnn_train \
    --do_gnn_test \
    --gnn_lr 5e-5 \
    --gnn_epoch 10000 \
    --gnn_n_layers 1 \
    --gnn_n_head 1 \
    --version Pretraining_GCN
```

**GAT Pre-training**
```
bash runs.sh pretraining_gnn Pretraining_GAT 0 term gat 5e-5 5000 1 2 
```
```
CUDA_VISIBLE_DEVICES=0 python -W ignore -u ./code/main.py \
    --gnn_model gat \
    --data_dir ./data/ \
    --mode term \
    --do_preprocessing \
    --do_gnn \
    --do_gnn_train \
    --do_gnn_test \
    --gnn_lr 5e-5 \
    --gnn_epoch 5000 \
    --gnn_n_layers 1 \
    --gnn_n_head 2 \
    --version Pretraining_GAT
```

**Graph-based QR Train (GCN)**
```
bash runs.sh graph-based_QR_train GCN_SOTA_model_train 0 term gcn 1 1 16 64 0.3 0.3 0 400 15.0 1e-5 1e-4 1024 3.0 cnn GCN_SOTA_model
```
```
CUDA_VISIBLE_DEVICES=0 python -W ignore -u ./code/main.py \
    --data_dir ./data/ \
    --mode term \
    --bert_dir ./data/bert/ \
    --bert_vocab ./data/bert/vocab.korean.rawtext.list \
    --do_gnn_qr \
    --do_gnn_qr_train \
    --gnn_qr_gnn_train \
    --gnn_qr_gnn_n_layers 1 \
    --gnn_qr_train_batch_size 16 \
    --gnn_qr_test_batch_size 64 \
    --gnn_qr_gamma 15.0 \
    --gnn_qr_gnn_lr 1e-4 \
    --gnn_qr_bert_drop 0.3 \
    --gnn_qr_gnn_drop 0.3 \
    --gnn_qr_gnn_model gcn \
    --gnn_qr_negative_sample_size 1024 \
    --gnn_qr_epochs 400 \
    --gnn_qr_query_model cnn \
    --negative_adversarial_sampling \
    --adversarial_temperature 3.0 \
    --save_name GCN_SOTA_model \
    --version GCN_SOTA_model_train
```

**Graph-based QR Train (GAT)**
```
bash runs.sh graph-based_QR_train GAT_SOTA_model_train 0 term gat 1 2 16 64 0.3 0.3 400 400 15.0 1e-5 1e-4 1024 3.0 cnn GAT_SOTA_model
```
```
CUDA_VISIBLE_DEVICES=0 python -W ignore -u ./code/main.py \
    --data_dir ./data/ \
    --mode term \
    --bert_dir ./data/bert/ \
    --bert_vocab ./data/bert/vocab.korean.rawtext.list \
    --do_gnn_qr \
    --do_gnn_qr_train \
    --gnn_qr_bert_train \
    --gnn_qr_gnn_train \
    --gnn_qr_gnn_n_layers 1 \
    --gnn_qr_gnn_n_head 2 \
    --gnn_qr_train_batch_size 16 \
    --gnn_qr_test_batch_size 64 \
    --gnn_qr_gamma 15.0 \
    --gnn_qr_bert_lr 1e-5 \
    --gnn_qr_gnn_lr 1e-4 \
    --gnn_qr_bert_drop 0.3 \
    --gnn_qr_gnn_drop 0.3 \
    --gnn_qr_gnn_model gat \
    --gnn_qr_bert_epochs 400 \
    --gnn_qr_epochs 400 \
    --gnn_qr_query_model cnn \
    --gnn_qr_negative_sample_size 1024 \
    --negative_adversarial_sampling \
    --adversarial_temperature 3.0 \
    --save_name GAT_SOTA_model \
    --version GAT_SOTA_model_train
```

**Only Graph-based QR Evaluation (GCN)**
```
bash runs.sh graph-based_QR_eval GCN_SOTA_model_test_threshold 0 term gcn 1 1 64 0.3 0.3 15.0 cnn GCN_SOTA_model 0.5
```
```
CUDA_VISIBLE_DEVICES=0 python -W ignore -u ./code/main.py \
    --data_dir ./data/ \
    --mode term \
    --dataset test \
    --bert_dir ./data/bert/ \
    --bert_vocab ./data/bert/vocab.korean.rawtext.list \
    --do_gnn_qr \
    --do_gnn_qr_test \
    --gnn_qr_gnn_n_layers 1 \
    --gnn_qr_test_batch_size 64 \
    --gnn_qr_gamma 15.0 \
    --gnn_qr_bert_drop 0.3 \
    --gnn_qr_gnn_drop 0.3 \
    --gnn_qr_gnn_model gcn \
    --gnn_qr_query_model cnn \
    --gnn_qr_threshold 0.5 \
    --save_name GCN_SOTA_model \
    --version GCN_SOTA_model_test_threshold
```

**Only Graph-based QR Evaluation (GAT)**
```
bash runs.sh graph-based_QR_eval GAT_SOTA_model_test_threshold 0 term gat 1 2 64 0.3 0.3 15.0 cnn GAT_SOTA_model 0.5 
```
```
CUDA_VISIBLE_DEVICES=0 python -W ignore -u ./code/main.py \
    --data_dir ./data/ \
    --mode term \
    --dataset test \
    --bert_dir ./data/bert/ \
    --bert_vocab ./data/bert/vocab.korean.rawtext.list \
    --do_gnn_qr \
    --do_gnn_qr_test \
    --gnn_qr_gnn_n_layers 1 \
    --gnn_qr_gnn_n_head 2 \
    --gnn_qr_test_batch_size 64 \
    --gnn_qr_gamma 15.0 \
    --gnn_qr_bert_drop 0.3 \
    --gnn_qr_gnn_drop 0.3 \
    --gnn_qr_gnn_model gat \
    --gnn_qr_query_model cnn \
    --gnn_qr_threshold 0.5 \
    --save_name GAT_SOTA_model \
    --version GAT_SOTA_model_test_threshold
```

**Graph-search + Graph-based QR Evalutaion (Pipeline / GCN)**
```
bash runs.sh pipeline GCN_SOTA_model_pipeline 0 term gcn 1 1 64 0.3 0.3 15.0 cnn GCN_SOTA_model 0.2 0.5
```
```
CUDA_VISIBLE_DEVICES=0 python -W ignore -u ./code/main.py \
    --data_dir ./data/ \
    --mode term \
    --dataset test \
    --bert_dir ./data/bert/ \
    --bert_vocab ./data/bert/vocab.korean.rawtext.list \
    --do_pipeline \
    --gnn_qr_gnn_n_layers 1 \
    --gnn_qr_test_batch_size 64 \
    --gnn_qr_gamma 15.0 \
    --gnn_qr_bert_drop 0.3 \
    --gnn_qr_gnn_drop 0.3 \
    --gnn_qr_gnn_model gcn \
    --gnn_qr_query_model cnn \
    --graph_search_threshold 0.2 \
    --gnn_qr_threshold 0.5 \
    --save_name GCN_SOTA_model \
    --version GCN_SOTA_model_pipeline
```

**Graph-search + Graph-based QR Evalutaion (Pipeline / GAT)**
```
bash runs.sh pipeline GAT_SOTA_model_pipeline 0 term gat 1 2 64 0.3 0.3 15.0 cnn GAT_SOTA_model 0.2 0.5
```
```
CUDA_VISIBLE_DEVICES=0 python -W ignore -u ./code/main.py \
    --data_dir ./data/ \
    --mode term \
    --dataset test \
    --bert_dir ./data/bert/ \
    --bert_vocab ./data/bert/vocab.korean.rawtext.list \
    --do_pipeline \
    --gnn_qr_gnn_n_layers 1 \
    --gnn_qr_gnn_n_head 2 \
    --gnn_qr_test_batch_size 64 \
    --gnn_qr_gamma 15.0 \
    --gnn_qr_bert_drop 0.3 \
    --gnn_qr_gnn_drop 0.3 \
    --gnn_qr_gnn_model gat \
    --gnn_qr_query_model cnn \
    --graph_search_threshold 0.2 \
    --gnn_qr_threshold 0.5 \
    --save_name GAT \
    --version GAT_SOTA_model_pipeline
```

