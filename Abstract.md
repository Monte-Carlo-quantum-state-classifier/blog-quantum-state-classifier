## Abstract

We created a model of communication protocol via a quantum channel. This protocol was designed to be efficient at classsifing states below a given error threshold, while maintaining quantum computing ressources within reasonable limits.

#### Transmission protocol



step 1: Alice sends Bob an ordered list of states through a classic channel

step 2: Alice sends Bob copies of each state in the same order through a quantum channel

step 3: Bob measures each of these states, applies measurement error mitigation if available, and creates a training probability distribution matrix (PDM)

step 4: Alices sends Bob an additional number of copies, also in the same order

step 5: Bob repeats step 3 for these states and obtains now a test PDM 

step 6: Using these PDMs, Bob performs a Monte Carlo simulation for increasing number of copies in order to determine  the minimal number of copies of each state that must be sent to maintain the error rate under a desired threshold. 

step 7: Bob classicaly conveys this minimal number to Alice

step 8: Alice sends Bob messages containing $n_s$ copies of each message state

step 9: Bob decodes the messages using a PDM based on the pooled training and test results


At step 9, Bob has the choice between two methods:

Either the predictor uses the pooled PDM obtained from the training and test data. 

Or steps 2 and 3 are skipped. Bob uses in step 6 an ideal PDM (theoretically deduced for a noise free device) instead of the trial PDM. He will obtain a higher value of the minimal number of copies to convey to Alice. The predictor uses now also this ideal PDM at step 9.

