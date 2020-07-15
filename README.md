# Appendix, Data Samples, Style Transferred Outputs and Source Code

This is the README of the supplementary material accompanying our EMNLP 2020 submission ("Reformulating Unsupervised Style Transfer as Paraphrase Generation"). If you are looking at the data file, read **Dataset Samples** and **Style Transferred Outputs**. If you are referring to the source code, look at **Source Code**.

## Appendix

We provided a detailed appendix in `appendix.pdf` in the root folder of this paper. This appendix has important hyperparameter choices, our survey of evaluations used in prior work, more comparisons with prior work, details of our dataset collection process, style-wise results of our system on our proposed dataset and lots of generated outputs.

## Dataset Samples

We present 1000 sentences from each our of our eleven diverse styles in `data_samples`. **WARNING**: These samples have not been filtered by profanity / toxicity and some sentences contain expletives or disturbing content. For the purpose of peer-review, we wanted to preserve our original data distribution as far as possible. We recognize this issue with the dataset and the potential harms it could have. We plan to discuss this at length before the eventual release and at the very least plan to add a datasheet discussing these limitations at length.

## Style Transferred Outputs

We style transfer each of the 1000 sentences in **Dataset Samples** to every other style and provide the outputs in `outputs`. These HTML files are best viewed using a browser. **WARNING**: These samples have not been filtered by profanity / toxicity and some sentences might contain expletives or disturbing content since the original dataset contained them. For the purpose of peer-review, we wanted to preserve our original outputs as far as possible. We recognize this issue and plan to discuss this at length before releasing these outputs publicly.

## Source Code

Note: **This source code is for reference only**. This codebase was used to train all the GPT2 models in our paper (both paraphrase and inverse paraphrase models). While the codebase has configurations for many complex preliminary experiments (which did not work out), we've set the configurations to the models eventually presented in the paper.

Note that this codebase does not contain dataset preprocessing or postprocessing scripts which are necessary to run experiments in the paper. We plan to make a proper Github release of the source code, datasets, model checkpoints, model outputs and a live demo of our system after the acceptance of the paper to ensure the results are fully reproducible and extendable.

Our source code is located in `gpt2_finetune`. Additionally, we use a modified version of the Transformers library in `transformers`. The only modification made is to run the style code model ablation study, which is a minor modification to `transformers/transformers/modeling_gpt2.py`.

#### Installation

This codebase uses PyTorch 1.2 installed with CUDA 9.2. All libraries can be installed from `pip` except `transformers`, for which we've used a local fork (install it using `pip install --editable .`).

### Description of Files

These descriptions are for files inside our main source code folder `gpt2_finetune/`. The best starting points are `schedule.py` and `hyperparameters_config.py`.

1. `args.py` - List of argument flags used for finetuning GPT2 with default values.
2. `data_utils.py`, `dataset_config.py` - Methods and classes to configure dataset and decide data preprocessing steps. 
3. `hyperparameter_config.py` - The list of hyperparameters used for training our diverse paraphrase model as well as the inverse paraphrase models.
5. `run_generation_dynamic.py` - The less preferred method to generate text using our models which integrates with our `Dataset` classes in `style_dataset.py`. The recommended way is using the `GPT2Generator` API in `inference_utils.py`. This script is called in `run_generation_gpt2_template.sh`.
6. `run_lm_finetuning_dynamic.py` - The primary script for finetuning models and evaluating models (in terms of perplexity). This script is called in `run_finetune_gpt2_template.sh` as well as `run_evaluate_gpt2_template.sh`.
7. `schedule.py` - Use the hyperparameters in `hyperparameter_config.py` to fill in bash scripts `run_*.sh` and schedule them on a SLURM cluster. Filled in examples of finetuning scripts are present in `examples/`
8. `style_dataset.py` - The Pytorch `Dataset` objects used for our paraphrase dataset and inverse paraphrase dataset.
9. `utils.py` - An important file containing the lowest level details of the training process.
10. `inference_utils.py` - This file has the `GPT2Generator` API wrapper which is used to perform generation. This API is very easy to use and loads any pretrained model. A user can easily specify the inputs as text in a list and the API will minibatch it and perform generation taking care of all the necessary preprocessing.

Finally, `saved_models`, `slurm-schedulers`, `logs`, `runs` are folders which contain experiment logs.
