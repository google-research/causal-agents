# CausalAgents: A Robustness Benchmark for Motion Forecasting using Causal Relationships

The causal agent labels are an additional attribute to the
[Waymo Open Motion Dataset](https://waymo.com/open/data/motion).
In addition to causal agent labels, we also release perturbed copies of the
Waymo Open Motion Dataset validation dataset, which serve as robustness
benchmarks to aid the research community in building more reliable and safe
models for motion forecasting. For more information, please see the paper
[CausalAgents: A Robustness Benchmark for Motion Forecasting using Causal
Relationships](https://arxiv.org/abs/2207.03586).

## Accessing the Data

In order to access the data, please go to
[https://www.waymo.com/open](https://www.waymo.com/open) and click on Access
Waymo Open Dataset, which requires a user to sign in with Google and accept the
Waymo Open Dataset license terms. After logging in, please visit
[https://console.cloud.google.com/storage/browser/waymo_open_dataset_causal_agents](https://console.cloud.google.com/storage/browser/waymo_open_dataset_causal_agents)
to download the labels.

## Dataset format

### Causal Agent Labels

The causal agent labels are released as a TFRecord of causal labels protos
([causal_label.proto](https://github.com/google-research/causal-agents/blob/main/protos/causal_labels.proto)).

The protocol buffer format for causal labels includes the following fields:

**CausalLabels**

-   scenario_id: The unique string identifier for the scenario.
-   labeler_results: A list of LabelerResults, one from each individual who
    labeled the datasets. Each scenario should have at least 5 individual
    labelers.

**LabelerResults**

-   causal_agent_ids: A list of causal agent id strings entered by the labeler
    in the submission box. The agent ids should correspond to the
    ["id" field](https://github.com/waymo-research/waymo-open-dataset/blob/master/waymo_open_dataset/protos/scenario.proto#L62)
    for the object Track in the Scenario proto. Note that the agent ids may be
    repeated in the list (they are not necessarily unique), and in a small
    number of cases the labeler may have entered a string that cannot be parsed
    as an agent id.

### Perturbed Datasets

We release four perturbed datasets:

1.  **RemoveNoncausal**
    -   Removes all non-causal agents in the dataset.
2.  **RemoveNoncausalEqual**
    -   Removes an equal number of randomly selected non-causal agents as there
        are causal agents in the scene. RemoveNoncausalEqual is meant to be a
        less aggressive form of RemoveNoncausal since it deletes fewer agents
        and it allows us to compare to RemoveCausal when controlling for the
        number of agents deleted.
3.  **RemoveStatic**
    -   Removes agents whose xyz positions do not change above a certain
        threshold (e.g. parked cars). Not all static agents are non-causal.
4.  **RemoveCausal**
    -   Removes all causal agents in the dataset, i.e., the complement of
        RemoveNoncausal.

Among them, we categorize both RemoveNoncausal and RemoveNoncausalEqual as
“non-causal” perturbations and we recommend these datasets as benchmarks for
measuring robustness in future work.

We include RemoveStatic and RemoveCausal as baselines. In our own work, we used
these datasets to sanity check model behavior (e.g. we expect the model to be
more sensitive to removing causal agents than non-causal or static agents).

To delete an agent from the original dataset, we set the
[valid bit](https://github.com/waymo-research/waymo-open-dataset/blob/master/waymo_open_dataset/protos/scenario.proto#L47)
of the corresponding agent to False for all time steps. Please double check that
any model you evaluate on the perturbed datasets correctly ignores all agent
state if the valid bit is false. (We verified this for all models we evaluated
in the paper).

We provide each perturbed dataset as sharded
[TFRecord](https://www.tensorflow.org/tutorials/load_data/tfrecord) files
containing
[WOMD tf.Example protos](https://waymo.com/open/data/motion/tfexample).

## License

This code repository is licensed under the Apache License, Version 2.0.

## Citation

If you use this data, please include the following citation:

> @article{roelofs2022causalagents, title={CausalAgents: A Robustness Benchmark
> for Motion Forecasting using Causal Relationships}, author={Roelofs, Rebecca
> and Sun, Liting and Caine, Ben and Refaat, Khaled S and Sapp, Ben and
> Ettinger, Scott and Chai, Wei}, journal={arXiv preprint arXiv:2207.03586},
> year={2022} }

## Disclaimer

This is not an officially supported Google product.
