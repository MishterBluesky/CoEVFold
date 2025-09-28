# CoEVFold <img width="45" height="43" alt="image" src="https://github.com/user-attachments/assets/fd3b848c-4565-4347-b881-813c0e27ae09" />


This is the homepage for CoEVFold co-evolutionary tools. The following links are to tools that will assist with co-evolution searching of your gene, whether with other proteins in 3D or if you are only interested in knowing the contact maps of input genes. It greatly depends on the GREMLIN covariance algorithm and MMSEQ2. Each tool has its uses.

1. To create a 2D Co-evolution contact map, and perform simple coevolution through MMSEQ2 of a user input protein sequence(s) use; Simple Coevolution https://colab.research.google.com/drive/1pTASZ-Ko5Q7TcTujXKCTtZ1j5_TPaZTi?usp=sharing

2. To find likely interfaces for homomeric multimerisation use: Homomeric Quaternary interaction finder https://colab.research.google.com/drive/1ujOg4abyLVxb64koOhxZ5qP5uFr-E65l?usp=sharing 

3. To plot co-evolution of heteromers of up to two different genes but with unlimited chains, and for more customisable co-evolution input use CoEVfold advanced (To plot co-evolution of heteromers https://colab.research.google.com/drive/17Ra7OJADodGlBuBtJRhRcPX6ksnsx9Dq?usp=sharing)

Related to this are also

ConservFold:https://colab.research.google.com/drive/1s7N6w2VEjadkJVS9bFzyOLaFZ3uImS0k?usp=sharing for Conservation determination by Weblogo as well as its better, infinite size complex and protein ensemble version; ConservFold2:https://colab.research.google.com/drive/1Lv-akfLE7kTCFCWaEyHAtsPCeXYD3xvH?usp=sharing

You may also find AlphaMatrix useful, a tool that has no coevolution but runs batches of alphafold as a means to look for AI structural predictions of interations https://colab.research.google.com/drive/1HU_KFWKyVLtz6u4MEMnZlXayaCYrQMw8?usp=sharing

Update: Conda has been changed to Mamba in these scripts! A small update to reduce crashes in recent colab versions.

Please cite the following paper when using this work:

"CoEVFold: an easy to use co-evolution on structure pipeline (C Graham, L Cremona and C Rodrigues 2025, Github)"
