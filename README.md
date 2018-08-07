## bilm

Using embedding from elmo https://github.com/allenai/bilm-tf . All codes are modified from elmo/bilm-tf.

### Installateion:

virtualenv -p python3 venv3

source venv3/bin/activate

pip install -e .

### Test Installateion:

	python -m unittest discover tests/

### Train:
	export CUDA_VISIBLE_DEVICES=0,1

	python bin/train_elmo.py \
	--train_prefix='tests/data/training_filtered/*' \
	--vocab_file tests/data/vocab_filtered.txt \
	--save_dir checkpoint \
	--config_file bin/resources/small_config.json

### Test:

	python bin/run_test.py \
	--test_prefix='tests/data/heldout_filtered/*' \
	--vocab_file testsdata/vocab_filtered.txt \
	--save_dir checkpoint

### Retrain:

	python bin/restart.py \
	--train_prefix='tests/data/training_filtered/*' \
	--vocab_file tests/data/vocab_filtered.txt \
	--save_dir checkpoint
	
Auto retrain a sequence of training dirs with span

	python bin/run_restart.py  \
	--vocab_file tests/data/vocab_filtered.txt \
	--save_dir checkpoint \
	--prefixes_dir tests/data/training_dir.paths \
	--span
	
### Get weights:

	python bin/dump_weights.py \
	--save_dir checkpoint \
	--outfile checkpoint/weights.hdf5

### Get vectors:
Provide a list of tokenized sentences

	python bin/get_vecs.py \
	--vocab_file tests/data/vocab_filtered.txt \
	--save_dir checkpoint \
	--input_text tests/data/tokenized_sentences.txt		

### Args:

1 vocab_file: 

	vocab_file.txt (not change for a model)

2 train_prefix: 

	dir1/* (if retrain dir2/*, dir3/* .... replace each retrain)

3 save_dir:

	checkpoint (same options.json, only n_train_tokens and n_epochs can be changed)

In options.json:

	batch_no = n_epochs*n_batches_per_epoch 

	n_batches_per_epoch = n_train_tokens/(batch_size*unroll_steps*n_gpus)

	epoch = batch_no/n_batches_per_epoch








