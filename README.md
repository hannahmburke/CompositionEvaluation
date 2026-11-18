# Service composition experiments

This folder contains models and experimental artefacts for evaluating privacy-preserving compliance verification of data-aware service compositions. The two subfolders cover experiments adapted from existing benchmark dataset, as well as a hand-crafted realistic loan application composition.

All experiments are fully-compliant, which ensures that the analysis explores the full search space before concluding that the model is compliant. 

## Experiments

- [SAP R/3 experiments](SAPR3_experiments/README.md) — Three service compositions adapted from processes in the SAP R/3 collection. Each composition has 3–5 services and covers varied service- and composition-level control flow, including atomic and linear processes, XOR choices, and nested XOR splits. The folder contains composition and service models, variable annotations, visualisations, certificates
- [Finance broker composition](FinanceBrokerComposition/README.md) — Hand-crafted example of a loan application workflow with outsourced services such as identity and credit checks, and bank quote requests, and in-house services performed by the finance broker. There are XOR, AND and cycles at both the composition level, and within different services. The folder contains the composition and individual service models, certificates, visualisations, and an expanded version of the model for comparison to the fully transparent approach. 
- `composition_results.csv` contains the timing results for all experiments. This compares our privacy-preserving approach based on generating and propagating certificates, to the fully transparent approach where the internal details of all services must be disclosed. 



Across the experiments, the privacy-preserving approach is generally within a similar order-of-magnitude as the fully-transparent approach, although it generally has a worse performance. When decomposing the total time of the privacy-preserving approach into certificate generation followed by certificate propagation, we find that the cost of each depends on the particular model; in some cases the larger domain the must be applied when generating certificates for each service has significant effect. In other cases, the certificate propagation is slower due to more complex output updates within the certificates, as opposed to directly applying the write operations in the fully-transparent approach. Lastly, there are a number of models where the privacy-preserving apporach outperforms, meaning the analysis benefits from the abstraction of particular services. Overall, the privacy advantage of our proposed approach justifies the occasional worse performance in contexts where privacy is a concern. 