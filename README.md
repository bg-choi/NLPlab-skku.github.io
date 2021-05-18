# Graph-based Query Reformulation System
This is PyTorch implementation of the Graph-based Query Reformulation System for Descriptive Queries of Jargon Words using a Dictionary.

```
pip install -r requirements.txt
```

**BERT embedding**
```
python main.py \
    --data_dir ../data/ \
    --mode term \
    --do_preprocessing \
    --do_bert_embedding \
    --bert_dir ../data/bert/ \
    --bert_vocab ../data/bert/vocab.korean.rawtext.list \
    --bert_max_seq_length 128 \
    --bert_batch_size 16 \
    --version 0
```

**Graph-search Model**
```
python main.py \
    --data_dir ./data/ \
    --mode term \
    --do_graph_search \
    --threshold 0.0 
```

**GNN Pre-training**
```
python main.py \
    --gnn_model gat \
    --data_dir ../data/ \
    --mode term \
    --do_preprocessing \
    --do_gnn \
    --do_gnn_train \
    --do_gnn_test \
    --gnn_lr 5e-5 \
    --gnn_epoch 10000 \
    --gnn_n_layers 1 \
    --gnn_n_head 2 \
    --version 0
```

**Graph-based QR Train**

```
python main.py \
    --data_dir ../data/ \
    --mode term \
    --bert_dir ../data/bert/ \
    --bert_vocab ../data/bert/vocab.korean.rawtext.list \
    --do_gnn_qr \
    --do_gnn_qr_train \
    --gnn_qr_gnn_n_head 2 \
    --gnn_qr_train_batch_size 16 \
    --gnn_qr_test_batch_size 64 \
    --gnn_qr_gamma 15.0 \
    --gnn_qr_gnn_lr 1e-4 \
    --gnn_qr_bert_lr 1e-5\
    --gnn_qr_bert_epochs 400\
    --gnn_qr_bert_drop 0.3 \
    --gnn_qr_gnn_drop 0.3 \
    --gnn_qr_gnn_model gat \
    --gnn_qr_negative_sample_size 1024 \
    --gnn_qr_epochs 400 \
    --gnn_qr_query_model cnn \
    --negative_adversarial_sampling \
    --adversarial_temperature 3.0 \
    --version 0
```

**Only Graph-based QR Evaluation**
```
python main.py \
    --data_dir ../data/ \
    --mode term \
    --bert_dir ../data/bert/ \
    --bert_vocab ../data/bert/vocab.korean.rawtext.list \
    --do_gnn_qr \
    --do_gnn_qr_test \
    --gnn_qr_gnn_n_head 2 \
    --gnn_qr_test_batch_size 1 \
    --gnn_qr_bert_drop 0.3 \
    --gnn_qr_gnn_drop 0.3 \
    --gnn_qr_gnn_model gat \
    --gnn_qr_query_model cnn \
    --version 0
```

**Graph-search + Graph-based QR Evalutaion**
```
python main.py \
    --data_dir ../data/ \
    --mode term \
    --bert_dir ../data/bert/ \
    --bert_vocab ../data/bert/vocab.korean.rawtext.list \
    --do_pipline \
    --do_gnn_qr_test \
    --gnn_qr_bert_drop 0.3 \
    --gnn_qr_gnn_drop 0.3 \
    --gnn_qr_test_batch_size 1 \
    --gnn_qr_gnn_n_head 2 \
    --gnn_qr_gnn_model gat \
    --threshold 0.0 \
    --version 0
```