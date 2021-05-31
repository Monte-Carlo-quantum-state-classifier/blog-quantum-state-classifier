# blog-quantum-state-classifier
Qiskit advocate s mentorship program

## Classification of quantum states with high dimensional entanglement using Qiskit


#### Introduction: 

High dimensional entanglement is proposed for transmission and memory storage of quantum information, e.g. possibly in future quantum imaging protocols.
 
Qiskit allows online access to real devices with different quantum readout fidelity $f$ and quantum volumes $QV$.

Qiskit therefore is an appealing playground for testing classification protocols on a set of highly entangled quantum states.

We therefore developed a model of communication protocol via a quantum channel. This protocol was designed to be efficient at classsifing states below a given error threshold while maintaining quantum computing ressources within reasonable limits.

#### Methods:

##### Set of highly entangled quantum states

The study state set $\Omega$ consisted of separable 5-qubit states $\omega_i,\; i = 1...20$, namely the 10 possible combinations of  $W\otimes\Psi^+$ and 10 possibles combinations of $\overline{W}\otimes\Phi^+$, where:

- $ W = \frac{1}{\sqrt{3}} \: (|1 0 0 \rangle \: +  |0 1 0 \rangle\: +  |0 0 1\rangle) $

- $ \overline{W} = \frac{1}{\sqrt{3}} \: (|0 1 1 \rangle \: +  |1 0 1 \rangle\: +  |1 1 0\rangle) $

- $ \Psi^+ = \frac{1}{\sqrt{2}} \: (|0 1 \rangle \: +  |1 0 \rangle) $

- $ \Phi^+ = \frac{1}{\sqrt{2}} \: (|0 0 \rangle \: +  |1 1 \rangle) $

##### Transmission protocol

![Protocol.png](attachment:Protocol.png)

1: Alice classicaly sends Bob an ordered list of states

2: Alice sends Bob an agreed number of copies of each state

3: Bob measures, applies error correction, and applies measurement rror mitigation

4: Bob creates a training probability distribution matrix (PDM)

5: Alices send s another agreeed number of copies and Bob creates a test PDM

6: Bob determines $n_s$ using Monte Carlo simulation for increasing number of copies

7: Bob classicaly tells Alice $n_s$

8: Alice sends Bob her message using $n_s$ copies of each state

##### Circuit composition 

The corresponding circuits were first established at the lower transpile level for an ideal fully connected device and run on the Qiskit Aer noise free simulator in order to establish the probability distribution matrix (PDM) for an ideal model $M_i$.


example of $W\otimes\Psi^+$ circuit:

![W_Psi_circuit.png](attachment:W_Psi_circuit.png)

corresponding $\overline{W}\otimes\Phi^+$:

![W_bar_Phi-circuit.png](attachment:W_bar_Phi-circuit.png)

Care was taken when creating a $\overline{W}$ state to reverse the qubit order in the "billard ball computer" sequence of gates which characterizes a $W$ state creation. The aim was to generate in noisy devices a different spectrum of correlated errors in the two types of state for better discrimination.

##### Experimental verification

The following experiments were achieved on eight 5-qubit superconducting computing devices available online in the IBM quantum experience (ibmq_athens, ibmq_belem, ibmq_lima, ibmq_ourense, ibmq_quito, ibmq_santiago, ibmq_valencia, ibmq_vigo):

- The filter for measurement error mitigation and the $f$ value at experimental period were first obtained.
	
- A corresponding circuit set was transpiled at level 2 into a frozen form and run on the real device.

- Two experimental PDM were obtained, with shot value set at 8192. The first PDM was reserved for serving as empirical model ($M_e$) for the QSC.

- For increasing shot number $n$, pseudo-random result distributions were drawn from the second PDM for Monte Carlo simulation. For each $n$ value, 1000 draws/state were made.

- These test results were submitted to a quantum state classifier (QSC) based on distance matrix computation using Scipy. As QSC metric, the euclidean squared distance and the Jensen-Shannon distance were both tested. For $M_i$ and $M_e$, the QSC error rates $\widehat{r_i}$ in function of $n$ were observed for each separable state $\omega_i$.

- This allowed calculating a shot number $n_s$ such that $\forall i  \widehat{r_i}\le \epsilon_s$. For this purpose, the curves were fitted using a $2^d$ degree polynomial Savitzky-Golay interpolation (SVI) on a 11-points moving window. A shot number $n_t$ was also obtained by SVI, such that $\widehat{r_m}\le \epsilon_t$, where $\widehat{r_m}$ is the mean error rate for all $\omega_i $ . The value of 0.0005 was choosen for $\epsilon_t$ because $\widehat{r_m}$ never exceeded 0.0005 for any $n_s$ shot values obtained when $\epsilon_s$ was set at 0.001.

![figCurve1.png](attachment:figCurve1.png)

- The entire process was repeated for circuit   with delay time $\delta t$  consisting of 256 identity gates inserted between state creation and measurement.

- Statistical method: statsmodels ordinary least square (OLS) was used taking the neperian logarithm of $n_s$ and $n_t$ as dependent variables.


 
#### Results:

- A preliminary computation revealed that $n_s$=16 and $n_t$=14 for the submitted $\Omega$ set in an ideal noise-free quantum device.


- The Monte Carlo simulation using the QSC worked fine in all configurations for all devices. 

![figCurve2.png](attachment:figCurve2.png)

- No significant difference was observed for $n_s$ and $n_t$ depending on the metric. The results obtained using euclidean squared distance were chosen for further statistics as it is more classical and strictly equivalent to the other well known Hellinger and Bhattacharyya distances.

  
- The Monte Carlo simulation revealed that a sufficiently low error rate can be attained at barely twice the shot values theoretically necessary in an ideal device.


- Lower values of $n_s$ and $n_t$ were generally observed :
  - using $M_e$ rather than $M_i$ for the classifier
  - when applying mitigation
  - whithout inserting $\delta t$ in the circuits


- Using OLS, an advantage of $M_e$ on $M_i$ was demonstrated for  $ln\;n_s$ and $ln\;n_t$ (p<0.01).



#### Conclusion:

The developed QSC confronted with a front-end empirical model was efficient in presence of a set of separable highly entangled quantum states. 

 
