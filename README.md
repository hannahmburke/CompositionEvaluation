# Service composition experiments

This folder contains models and experimental artefacts for evaluating privacy-preserving compliance verification of data-aware service compositions. The two subfolders cover experiments adapted from existing benchmark dataset, as well as a hand-crafted realistic loan application composition.

All experiments are fully-compliant, which ensures that the analysis explores the full search space before concluding that the model is compliant. 

## Experiments

- [SAP R/3 experiments](SAPR3_experiments/README.md) — Three service compositions adapted from processes in the SAP R/3 collection. Each composition has 3–5 services and covers varied service- and composition-level control flow, including atomic and linear processes, XOR choices, and nested XOR splits. The folder contains composition and service models, variable annotations, visualisations, certificates
- [Finance broker composition](FinanceBrokerComposition/README.md) — Hand-crafted example of a loan application workflow with outsourced services such as identity and credit checks, and bank quote requests, and in-house services performed by the finance broker. There are XOR, AND and cycles at both the composition level, and within different services. The folder contains the composition and individual service models, certificates, visualisations, and an expanded version of the model for comparison to the fully transparent approach. 
- `composition_results.csv` contains the timing results for all experiments. This compares our privacy-preserving approach based on generating and propagating certificates, to the fully transparent approach where the internal details of all services must be disclosed. 

## Results summary

Across the experiments, the privacy-preserving approach is typically one or two orders of magnitude slower than the process-transparent approach. It is worst for the U10 experiments, where the certificate generation step suffers from the larger input domain. This cannot be entirely avoided, as certificate generation must consider the full input domain in order to remain agnostic to other services in the composition. 
Nevertheless, a key focus for performance improvement is to improve the scalability with respect to domain size, potentially through more optimised CSP solving techniques, or exploring SMT-based approaches. 

On the other hand, the finance broker performance is comparable to the process-transparent method, despite many of the model's variables having an input domain size in the hundreds or thousands. Unlike the SAP R/3 experiments, where service boundaries were drawn randomly, the services in this scenario are relatively self-contained in how they affect the obligations. This results in minimal branching in the service-level analysis tree, which reduces the number of CSP calls over the large domain space, and thus the certificate generation step is less affected. 

With respect to certificate propagation, the results show this is a small portion of the total time for the privacy preserving approach, as it operates on a smaller composition model compared to the expanded model in the process-transparent approach. However, some cases have increased overhead, possibly due the increased effort of reasoning over certificate data structures, which is more difficult than having direct access to the write operations. Consequently, performance improvements could be achieved through optimisations of the certificate data structures. 

Overall, while the results show that performance cost is incurred when treating each service in a composition as a black-box for compliance checking, our proposal offers important privacy benefits. 
This trade-off against performance is justified in scenarios where privacy is a primary concern. 
