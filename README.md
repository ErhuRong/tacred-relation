Position-aware Attention RNN Model for Relation Extraction
=========================

This repo contains the *pytorch* code for paper [Position-aware Attention and Supervised Data Improve Slot Filling](https://nlp.stanford.edu/pubs/zhang2017tacred.pdf).

**About TACRED data**: Please note that we are still in the process of licensing TACRED with LDC. For completeness this repo only contains sampled data from TACRED. If you'd like to receive email notifications once TACRED is ready for download, please [**fill out this form**](https://goo.gl/forms/nO70omzVyRNbDXt53).

## Requirements

- Python 3 (tested on 3.6.2)
- PyTorch (tested on 0.1.12)
- unzip, wget (for downloading only)

## Preparation

First, download and unzip GloVe vectors from the Stanford website, with:
```
chmod +x download.sh; ./download.sh
```

Then prepare vocabulary and initial word vectors with:
```
python prepare_vocab.py dataset/tacred dataset/vocab --glove_dir dataset/glove
```

This will write vocabulary and word vectors as a numpy matrix into the dir `dataset/vocab`.

## Training

Train a position-aware attention RNN model with:
```
python train.py --data_dir dataset/tacred --vocab_dir dataset/vocab --topn 1000 --id 00 --info "Position-aware attention model"
```

Use `--topn 1000` to finetune the top 1000 word vectors only. The script will do the preprocessing automatically (word dropout, entity masking, etc.).

Train an LSTM model with:
```
python train.py --data_dir dataset/tacred --vocab_dir dataset/vocab --topn 1000 --no-attn --id 01 --info "LSTM model"
```

Model checkpoints and logs will be saved to `./saved_models/00`.

## Evaluation

Run evaluation on the test set with:
```
python eval.py saved_models/00 --dataset test
```

This will use the `best_model.pt` by default. Use `--model checkpoint_epoch_10.pt` to specify a model checkpoint file. Add `--out saved_models/out/test1.pkl` to write model probability output to files (for ensemble, etc.).

## Ensemble

Please see the example script `ensemble.sh`.

## License

All work contained in this package is licensed under the Apache License, Version 2.0. See the included LICENSE file.
