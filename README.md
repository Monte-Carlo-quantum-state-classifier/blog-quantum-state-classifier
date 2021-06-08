# Qiskit quantum state classifier
Pilote project testing by a Monte Carlo simulation a quantum state classifier based on distance matrix computation. The classifier is applied on a set of separable highly entangled quantum states and uses either an ideal model of measurement count distributions or a front-end empirical model.

Details of the project can be found in [this notebook](https://github.com/Monte-Carlo-quantum-state-classifier/blog-quantum-state-classifier/blob/gh-pages/1_project_description.ipynb)

## Abstract

We created a model of communication protocol via a quantum channel. This protocol was designed to be efficient at classsifing states below a given error threshold, while maintaining quantum computing ressources within reasonable limits.

#### Transmission protocol



1: Alice sends Bob an ordered list of states through a classic channel

2: Alice sends Bob copies of each state in the same order through a quantum channel

3: Bob measures each of these states, applies measurement error mitigation if available, and creates a training probability distribution matrix (PDM)

4: Alices sends Bob an additional number of copies, also in the same order

5: Bob repeats step 3 for these states and obtains now a test PDM 

6: Using these PDMs, Bob performs a Monte Carlo simulation for increasing number of copies in order to determine  the minimal number of copies of each state that must be sent to maintain the error rate under a desired threshold. 

7: Bob classicaly conveys this minimal number to Alice

8: Alice sends Bob messages containing $n_s$ copies of each message state

9: Bob decodes the messages using a PDM based on the pooled training and test results


At step 9, Bob has the choice between two methods:
- Either the predictor uses the pooled PDM obtained from the training and test data. 
- Or steps 2 and 3 are skipped. Bob uses in step 6 an ideal PDM (theoretically deduced for a noise free device) instead of the trial PDM. He will obtain a higher value of the minimal number of copies to convey to Alice. The predictor uses now also this ideal PDM at step 9.
