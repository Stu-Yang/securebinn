# SecureBiNN

## Overview

The paper proposes SecureBiNN, a novel three-party secure computation framework for evaluating privacy-preserving binarized neural network (BiNN) in semi-honest adversary setting.
In SecureBiNN, three participants hold input data and model parameters in secret sharing form, and execute secure computations to obtain secret shares of prediction result without disclosing their input data, model parameters and the prediction result.
SecureBiNN performs linear operations in a computation-efficient and communication-free way. For non-linear operations, we provide novel secure methods for evaluating activation function, maxpooling layers, and batch normalization layers in BiNN.
Communication overhead is significantly minimized comparing to previous work like XONN and Falcon.
We implement SecureBiNN with tensorflow and the experiments show that using the Fitnet structure, SecureBiNN achieves on CIFAR-10 dataset an accuracy of 81.5%, with the communication cost being 16.609MB and the runtime being 0.527s/3.447s in the LAN/WAN settings.
More evaluations on real-world datasets are also performed and other concrete comparisons with state-of-the-art are presented as well.

## Illustration of this project

In theory, SecureBiNN supports arbitrary division of data and models among the three participants. However, for simplicity,
we suppose three participants are data owner, model owner and
a trusted third party. The data owner seperate the data to other participants, and the model owner seperates the shares of the model.

## Requirments

- python == 3.8
- Tensorflow == 2.4.1
- numpy == 1.19.0

## How to run this project ?

First fill in the relevant settings in `role/config.json` according to the actual situation, including the `ips` and the `ports` of three participants. 

```shell
# /root/workplace/securebinn/role/config.json
update line 17: "model_path": "models/Network-A.h5"
update line 24: "archive_path":"archives/"
```

Then, run

```shell
root@name:~/workplace/securebinn# conda activate securebinn
(securebinn) root@name:~/workplace/securebinn# python make_roles.py
```

to generate three files representing three different participants. For each participant, execute

```shell
>>> split into 3 terminals
(securebinn) root@name:~/workplace/securebinn/role_0_data_owner# python main.py
(securebinn) root@name:~/workplace/securebinn/role_1_model_owner# python main.py
(securebinn) root@name:~/workplace/securebinn/role_2_ttp# python main.py
```

to run the SecureBiNN.
The report message will be returned to data owner.

There are a example message below:

```shell
# (securebinn) root@yangpeng:~/yp_workplace/securebinn/role_0_data_owner# python main.py
ecurebinn/role_0_data_owner# python main.py
Connect to 127.0.0.1:45002 successfully.
Connect to 127.0.0.1:45004 successfully.
Seperate model
Seperate data
Dataset mnist loaded.
60000 training samples, 10000 testing samples.
The shape of the answer: (1, 10)
Evaluate begin:
Iter 0 begin.
Iter 1 begin.
Iter 2 begin.
Iter 3 begin.
Iter 4 begin.
Iter 5 begin.
Iter 6 begin.
Iter 7 begin.
Iter 8 begin.
Iter 9 begin.
N_iter: 10
Accuracy: 100.0000 %
Total:
Time: 0.281s, Comm: 0.051MB
Per iteration:
Time: 0.028s, Comm: 0.005MB
Per input:
Time: 0.028s, Comm: 0.005MB
```

```shell
# (securebinn) root@yangpeng:~/yp_workplace/securebinn/role_1_model_owner# python main.py
Connect to 127.0.0.1:45000 successfully.
Connect to 127.0.0.1:45005 successfully.
Seperate model
0: dense_1 coding with 16 bits.
1: batch_normalization_17 coding with 16 bits.
2: activation_18 coding with 16 bits.
3: dense_2 coding with 16 bits.
Seperate data
Evaluate begin:
Iter 0 begin.
Iter 1 begin.
Iter 2 begin.
Iter 3 begin.
Iter 4 begin.
Iter 5 begin.
Iter 6 begin.
Iter 7 begin.
Iter 8 begin.
Iter 9 begin.
```

```shell
# (securebinn) root@yangpeng:~/yp_workplace/securebinn/role_2_ttp# python main.py
Connect to 127.0.0.1:45003 successfully.
Connect to 127.0.0.1:45001 successfully.
Seperate model
Seperate data
Evaluate begin:
Iter 0 begin.
Iter 1 begin.
Iter 2 begin.
Iter 3 begin.
Iter 4 begin.
Iter 5 begin.
Iter 6 begin.
Iter 7 begin.
Iter 8 begin.
Iter 9 begin.
```