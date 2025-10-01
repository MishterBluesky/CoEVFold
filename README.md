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


Prior to publication, please see the below notes on the software, which we will submit to journals after some revision shortly.

CoEVFold: an easy to use co-evolution on structure pipeline
Chris LB Graham1,*, Liam Cremona  and Chris Rodrigues2 *

1Department of Life Sciences, University of Warwick, Coventry, UK
*To whom correspondence should be addressed.

ABSTRACT

Co-evolution is the backbone of protein folding and interaction prediction, on which large language models, in tandem with protein datasets can predict structure. Currently many scientists are using these LLM model predictions as hypothesis generators or use the protein-protein interactions they predict as evidence for interaction in vivo. However, the empirical reasoning for prediction of the folding, through alignment of coevolved residues is often lost. Here we present a method to visualize that co-evolution again, with a pipeline that uses basic principles of co-evolution including a GREMLIN derived direct-coupling algorithm and MMSEQ2 to provide the alignments to that algorithm. The pipeline can overlay co-evolution onto a single protein as well as apply and discover quaternary co-evolution interactions of a protein with itself or for hetero or homo -mmer resolution. It does this either using the proteins amino acid sequence, or optionally user input structures. We found that this easy-to-use pipeline ‘CoEVFold’ was able to predict quaternary co-evolution interactions of a new protein multimer, as well as existing protein multimers, therefore is a viable method of more robust hypothesis generation. Ensuring the notebooks are easily accessible, we applied the tools to our own system in Bacillus subtilis, as well as other notable interactors in Escherichia.coli, a phage tail protein and the eukaryote Saccharomyces cerevisiae, however it should work on any system with sufficient genome phylogeny availability.

Availability: The code is available on github and online at https://colab.research.google.com/drive/1MSSvNTq7KZ4Lr0XTz89vUuK-J3xOTzwS?usp=sharing and Github.

Supplementary information: Supplementary data is available via Figshare and supplementary materials.

INTRODUCTION

Co-evolution between amino acids within a protein, allows for detection of amino interconnections which reveal a 3D landscape viewable via a contact map, similar to that of the contact map produced by the overall protein structure. This has been used to contribute towards folding prediction(s) and has been shown to be essential for protein folding by large language models (LLMs;1). As well as allowing for discovery of interactions of a protein with itself, co-evolution can also be used for predictions of protein-protein interactions in multimer complexes with two proteins. The GREMLIN algorithm, established by the Baker lab(1,2) has been shown to function well as a co-evolution predictor, however current tools are losing support and there has not been an advance in new co-evolution pipelines which re-introduce direct coupling analysis, that is the basic measure of two amino acids co-variation.  Protein homomers, predicted by LLMs are a contrast to heteromers or monomers in that a new level of protein-protein connection occurs not only within a protein and to other proteins, but to themselves beyond the initial tertiary structure(3). This quaternary interaction can be difficult to filter or isolate, however, it has been studied before with success(4). Co-evolution is not always representative of a contact, for example it can also be regions of shared signal sequences, or via an intermediate protein.

There are two notable uses of co-evolution, firstly that of predicting the sites of interfaces, and discovering multimers, both of which can then be confirmed through mutagenesis or other biochemical means, such as cryo-EM and crystallography. Secondly, co-evolution on a systems biology scale can be used to look at a network of interactions, to determine which genes may interact with one another, and inform on larger mechanisms or regulatory circuits. 

Co-evolution is the evolution of one thing relative to another. In the respect of amino acids in proteins, this could be the evolution of two amino acids within a protein sequence to enable a stabilisation of the protein. It is generally theorised and has been shown by the success of learning models, that a highly co-evolved amino acid pair, are likely also in physical contact.  An example of this can be found below: in Figure 1A, a sequence alignment of a hypothetical protein, across a set of seven species indicates an arginine (R) aspartate (D) pairing, this same pairing when amino acid R evolves to Lysine K, a reciprocal amino acid change takes place to allow for stabilisation and folding (Figure 1B) - thus a co-evolution is occurring. The ability to predict a co-evolution, or likelyhood of amino acid y changing, dependent on amino acid x, is a co-evolution score. This score can be represented in a 2D grid (Figure 1C), to give us a likely 3D structure (Figure 1D).
 
<img width="385" height="263" alt="image" src="https://github.com/user-attachments/assets/3e92aa56-2865-4e92-bdcd-2c3182ea45b8" />

Figure 1. Basic concepts in co-evolution 
A Multiple sequence alignment, B Contact interface as observed, C contact map of co-evolutionary scores D Hypothetical overall 3D structure.

Our tool CoEVFold(Figure 2) works in the same way. It first relies on MMSEQ2 (5) to create an alignment of amino acids based on similarity from the input sequence. It then relies on the well-established simple direct coupling analysis learning algorithm (GREMLIN)(1), to come up with the scores for each amino acid - amino acid contact score in a grid. This can also be applied to amino acids in other proteins to show coevolving pairs in heteromeric systems. The concept of the addition of multiple proteins in 3D space, homomeric or multimeric is collectively known as a quaternary interaction(3,4). 

We have designed a series of Jupyter notebooks that form an easy to use pipeline for co-evolution study. Beyond this, we have then allowed users to input their own input protein structures from LLMs or other predicted and physical structures which can be used as a basis for the plotting of co-evolution between proteins in homomers, multimers, and monomers as well as at a gene network level. These tools, based on existing co-evolution principles and published work, are the natural next steps in understanding the empirical evolutionary evidence for the LLM structure outputs. 

We provide a pipeline to find co-evolution in a gene encoding a protein and then compare protein monomer contact maps and multimer contact maps to find contact points in co-evolution explained by homo-multimers, thus facilitating another way to understand the evolutionary evidence for multimer formation in proteins. For this we provide key examples of novel and known heteromers as well as homomers in bacteria, yeast and phage. This comes with a set of pipelines for different analyses dependent on system type, the pipeline is known as ‘CoEVFold’.

RESULTS

Navigating to CoEVFold; similar to ColabFold(6) a user input sequence or sequences are aligned by MMSEQ2 using user defined custom parameters, which after alignment provides a .a3m file. The .a3m file is then converted to a fasta file. Following from this the user has two options: heteromeric co-evolution or homomeric co-evolution.

Heteromer mode: Quaternary co-evolution and the study of hetero-multimers 

A user may input a model of any size, as long as there are two encoding genes interacting. For example, a heteromer of four of protein A, and two of protein B. These show intergene co-evolution interactions are imposed onto the heteromer structure as a .pse file (Figure 2) and the output shown.

In Figure 2A, we input two proteins from one species to the multiple sequence alignment eg: pbpB and ftsW, and the aligned species can be used to calculate coevolution, with consistent amino acid changes occurring in both proteins for each species. The ability to predict the evolutionary change in amino acid y of protein b (ftsW), using amino acid x in protein a (roda), is also co-evolution. The strength of this evolutionary interaction is effectively the co-evolutionary score for each amino acid. This allows for prediction of multimeric complexes in any species, and determination of where this interaction is most likely. In this example we use heterodimer B. subtilis proteins as FtsW and PbpB(7–9), as well as the multiprotein yeast VO V-ATPase complex (10) as examples (Figure 2.) It is possible to plot the co-evolution of multiple proteins, provided there is sufficient number of alignments to indicate the coevolving residues within the genome (only of within the input genes) of a species. The ability to predict these interactions using direct coupling analysis, has been already established(11,12) , therefore we do not present new results here.

 <img width="259" height="403" alt="image" src="https://github.com/user-attachments/assets/6780612b-b244-4dd7-a826-f8d263cf0ae9" />


Figure 2. CoEVFold pipeline
A MMSEQ2 alignment and a3m production based on query sequence – B subtilis FtsW-PbpB 50% coverage. B Co-evolution performed using GREMLIN on transformed a3m file.  C user input structure, or ColabFold structure is used to impose the co-evolution. Red dashed lines = Co-evolution at user thresholds (12A, 1.5 GREMLIN score). D Co-evolution based on query pdb input PDB id 7TA0 Cryo-EM structure of bafilomycin A1 bound to yeast VO V-ATPase 50% coverage E Dashed lines represent co-evolution links at user thresholds (12A, 1 GREMLIN score) Each colour represents a unique interchain quaternary link type.

Homomer mode: Quaternary evolution to self

This mode looks for quaternary interactions by co-evolution for a gene to itself.  Through use of intermediary tools, such as the widespread protein folding prediction tools AlphaFold (13) scientists and members of the public are able to predict monomeric protein structure with very high accuracy, and one can compare the co-evolution of a protein or its ‘co-evolutionary 2D contact map’ as visualised in Fig. S1c, with a known ‘physical contact map’ of a structure (or high accuracy AI predicted protein structure, such as monomeric Alphafold3 proteins) with various threshold distances. In our tools, we take advantage of this to find interactions that are not explained by monomeric interactions, but only by multimers - ‘Multimer explained co-evolution’.
 
Many tools are now capable of predicting the structures of homomeric multimer assemblies, therefore plausibly the physical backbone to backbone contact maps of these multimers can be compared to the co-evolution maps, and the ‘best fitting’ outcome chosen. This is in addition to pTM and pLDTT scores of LLMs which can show the stability and confidence that various artificial intelligence tools have in their predictions can give a user a guide to a potential true structure. Through this, one can discern the likelihood, albeit manually( but based off co-evolution data), that a protein is a multimer or not, and where those multimeric interactions may occur from an evolutionary standpoint. 

There two sub-modes within this homomer search mode:

a.	Guided quaternary evolution – Here the user provides the input sequence, which is aligned using MMSEQS2 (Figure 3a) and co-evolution is calculated as previously described (Figure 3b), then the user provides both a monomeric .pdb file of the protein, as well as a multimeric .pdb file predicted by a structural prediction tool such as AlphaFold or TrRosetta (Figure 3c/f). The program finds user thresholded co-evolution only explained by the contact maps of the multimer, and plots these on to the structure as a. pse. (Figure 3G). In multimer verification and analysis mode, the 3D contact map of the multimer is also compared (Figure 3H). The co-evolution score is then plotted on the same contact map for both inputs for plotting graphically, red indicating multimer interactions and blue monomer interactions. 
b.	Unguided quaternary evolution – Here the user provides only a monomeric pdb file of the protein; They can search for unexplained co-evolution, visualised on a 2D contact map (Figure 3E). This co-evolution is either conformational or phylogenetic co-evolution noise, or alternatively, the basis for a quaternary interaction in homomers. The 12A contact map of the protein is then used to filter out only co-evolution not explained in this 3D map, this gives a set of residues and their scores, which are yet to be explained, which if there is a multimer, especially in areas of high clustering, may be where a multimer forms.

<img width="424" height="457" alt="image" src="https://github.com/user-attachments/assets/236c7655-dd16-4ed2-9c95-fa30cf808f57" />

 
Figure 3. Quaternary interaction methodology for homomeric quaternary interactions
A MMSEQ2 alignment and a3m production based on query sequence – B subtilis motor domain SpoIIIE 50% coverage B Co-evolution performed using gremlin on transformed a3m file.  C User input monomer/lower order multimer and higher order multimer (F), creating contact map (including intermolecular links). D Comparison of monomer contact map with co-evolution, showing monomer/low order unexplained coevolution E. F/G Refinement of search area to co-evolution only present in input higher order multimer, red- multimer explained evolution , purple – monomer explained coevolution, , H Optional thresholds of multimer only explained contacts are displayed in a prepared .pse file.on input multimer.

Having created the first iteration of the pipeline, in Figure 4 we set out to confirm that co-evolution pairs not explained by monomerization/lower order structures could be caused by higher order multimerization of a protein as previously described in B. subtilis sporulation genes (4), we took an existing known tetramer of E. coli, MutS (14), and imagined that we did not know it was a tetramer. Instead comparing not only the monomer co-evolution, but the existing crystal structure of the dimerised structure currently available. We calculated the unexplained co-evolution of MutS beyond the dimer form when the contact maps were overlayed (Figure 4A and found that a major cluster of co-evolution remaining was at its C terminus This unexplained co-evolution, made through contact map subtraction agreed with the existing tetramer paper, which looks at MutS tetramer formation through in vitro mutagenesis of its C terminus (Figure 4). MutS the C terminus D835 and R840, as well as C terminal cut-off mutants have already shown to be responsible for higher order multimerization, with their absence or change leading to loss of function. This agreed with the theory that quaternary co-evolution can be resolved from lower order structures and co-evolution.

We then investigated the E.coli DiaA protein, which is also a known tetramer (15), (Figure 4B) and again found that at regions of high co-evolution not explained by the monomer (Figure 4Biv) interactions were present in the known structure, and integral to the formation of the tetramer, which we highlight visualised as spheres in the structure, that provide a connection between units. We therefore postulate that co-evolution could have been used in these cases to detect the most important residues in a multimeric interaction, and to suggest new higher oligomeric forms. There were additional interactions seen, however by changing the contact map threshold of the input pdb folder and looking for regions of high co-evolution prevalence not explained by a monomer, the interactions necessary for quaternary interactions become clearer dependent on user preference and in 3D inspection of potential hypothetical oligomers.

<img width="415" height="440" alt="image" src="https://github.com/user-attachments/assets/6ec63e38-4e86-471c-ae85-ab768cd890f6" />

 
Figure 4. Unexplained Co-evolution searching yields similar residues to in vitro searches 
A MutS, i MMSEQ2 sequences 50% coverage, ii Co-evolution (GREMLIN) Iii user input dimer, creating contact map iv Overlay v Unexplained contacts vi MutS dimer predicted by AlphaFold. Black box, mutated residues shown by in vitro experiments to be responsible for tetramer formation. B i MMSEQ2 sequences 50% coverage, ii Co-evolution (GREMLIN) Iii User input AlphaFold monomer, creating contact map iv Overlay v unexplained contacts vi DiaA tetramer predicted by AlphaFold. Black box, mutated residues shown by in vitro experiments to be responsible for tetramer formation red and green regions interact, as in co-evolution.

However, we also hoped to use this technique to try and identify new multimers, and not only repeat existing data. Therefore alongside existing tools, with the assistance of protein structure prediction in order to verify that it is possible to identify new multimers, we used the structure of known multimers to be compared to monomer structures to re-affirm that unexplained co-evolution maps of those multimers to these regions of contact (Figure 5). We tested the known multimers; SpoIIIE motor domain (B. subtilis inferred by FtsK homologue (16)), MlaD (Escherichia coli)(17) and Tequatrovirus T4 phage tail protein 4UXG(18). We highlight co-evolution contacts only corresponding to contact maps of the multimers, and not the monomers, these can be visualised when zoomed in, as the red contacts on the contact map, juxtaposing the purple contacts which are also present in the multimer. (Figure 3 A, B, C) Each protein had co-evolution interactions that are only visible in the multimer, and that agreed with the known multimer, with a co-evolution cut-off of 2 (unadjusted GREMLIN algorithm Z score) and contact map threshold of 12A. However, not all co-evolution could be explained by multimers. This has been speculated on and seen before (1,3,4), these regions of unexplained co-evolution could cause users to believe a quaternary interaction is occurring when none is present. These interactions may occur through shared signalling pathways causing co-evolution, shared proteins, but could also be the result of noise dependent on the relative scoring chosen by the user, as well as input sequences. 

<img width="424" height="408" alt="image" src="https://github.com/user-attachments/assets/d7e6602f-1eda-414d-ace6-c81210172af0" />

 
Figure 5. Multimer co-evolution and  ipTM comparisons, SpoIIIE, MlaD, Tail Fibre
A SpoIIIE (FtsK homologue) B. subtilis motor domain Alphafold3 prediction, i MMSEQ2 sequences 50% coverage, ii Co-evolution (GREMLIN) iii Multimer only contacts (red), monomer (blue) and both (purple) iv Contacts in 3D between chains above GREMLIN score 1. B MlaD Escherichia coli monomer contrast to crystal PDB ID , C Phage tail PDB ID 4UXG  D. AlphaFold3 ipTM score of each protein oligomer at user input oligomeric states.

In addition whilst troubleshooting our method, when comparing crystal structures, issues arose with direct comparisons of a monomer self-interaction in that there are small differences in predicted structures at the tertiary level, meaning monomers, or an array of monomers in alternative conformations dependent on crystal structure, can have a larger interaction surface for self-contact than expected, before any intermolecular or quaternary contact occurs. This means, that care must be applied in discovering multimer interactions, to look for significant clusters of new protein interface, especially in small proteins where new contacts are more likely to occur due to large intra-protein shifts in conformation. Therefore as a final step of inter-molecular interaction selection, in each case we also plot the co-evolution in 3D using an angstrom cut-off applied between the backbone of chains in the hypothetical multimer, meaning that if this tool was used to confirm or discover interactions between subunits, only interactions that only occur between subunits within the threshold would be annotated, and not potential differences in tertiary interactions, where interactions may be extremely far apart i.e. above 30 Angstroms. 

In order to remedy both of these issues, we therefore recommend, (and have tested ourselves) (Figure 5/6) using structure prediction tools and the ipTM or pTM they provide as an additional guide. In Figure 5D, we carried out a series of ipTM score to oligomeric state studies(13), and found that these in general revealed the highest ipTM at the correct multimerization state, which can act as an additional guide. Whether the user bolsters co-evolution evidence with the structural predictions or vice versa, the presence of regions of co-evolution interaction, only explained by these multimers allowed for accurate re-production of existing crystal structures, using current structural prediction tools and co-evolution. Again however, this isn’t always straightforward. In Figure 6, we chose a large assembly of the Bacillus subtilis protein SpoIIIAG(19). This assembly was not predictable in the state of the art AlphaFold3 using public accounts due to character limits. We found that the ipTM was highest for a hexameric state (Figure 6A). However when the co-evolution contacts are compared to known structure of the 30-mer state (Figure 6B vs 6D), it is clear that these contacts are the same, if the hexameric patterning is able to continue. It could be possible for others to use co-evolution to predict regions more likely to be the interaction surface of these larger assemblies, with predictive structures of ‘units’ acting as a guide.

<img width="361" height="514" alt="image" src="https://github.com/user-attachments/assets/d72b5035-87dc-47c3-8f68-538f4fd4563b" />

 
Figure 6. Multimer co-evolution and  ipTM comparisons, SpoIIIAG

A. AlphaFold3 ipTM score of Bacillus subtilis SpoIIIAG oligomeric states 2-23. MMSEQ2 sequences B Co-evolution of hexamer explained (GREMLIN) multimer only contacts (red) , monomer (blue) and both (red) C Co-evolution of SpoIIIAG residues available in pdb structure file. D Co-evolution of 30-mer explained (GREMLIN) multimer only contacts (red) , monomer (blue) and both (red). E Quaternary homomer co-evolution interactions on each structure visualized (above Z score 1)


CONCLUSION

We applied established co-evolution prediction methods to generate 2D contact maps of genes of interest using a combination of MMSEQS2 and the GREMLIN direct coupling analysis algorithm. These were then mapped onto protein structures predicted by AlphaFold or user-provided models to create a pipeline of co-evolution visualisation. Our pipeline has been shown to predict in vivo/in vitro results, both for homomeric multimerization and heteromeric multimerization. Co-evolution has already shown value in studying protein complexes and has been reported previously, however, our goal was to develop a streamlined pipeline that, together with the examples we provide, encourages researchers to explore both heteromeric and homomeric interactions in the context of co-evolution. By doing so, we aim to reduce the “black-box” nature of unguided AI predictions, giving users tools to verify or provide supporting evidence for potential multimeric links with the context of co-evolution. Ultimately, we hope that our software grounded in established algorithms will help shed light on complex structures predicted by new AI tools and serve as another means of validating their accuracy, but also facilitate new hypothesis generation.

METHODS

Prediction of protein structures

All protein structures and multimers listed, unless otherwise stated as an existing pdb structure were created using submissions to the AlphaFold3 server with no modifications. B subtilis SpoIIIAG was predicted using residues 40-189. The B subtilis SpoIIIE motor domain was predicted using residues 353-789.

CoEVFold Heteromer and Homomer modes.

The user either inputs a protein pdb file, or a sequence which is then aligned using MMSEQS2 using user defined parameters. Within CoEVfold the sequences are scored for co-evolution, using the GREMLIN direct coupling covariance method for 100 iterations using default parameters. Once a co-evolution contact map is calculated the mode then changes the following step. In homomer mode co-evolution is scored for each amino acid pair (users are only able to give an input sequence) and users may provide a pdb of the monomer or lower order multimer (for example from AlphaFold database) and optionally that of the multimer which will then either take the user through guided or unguided mode as described. These files must be trimmed to have the same amino acid length as the input sequence. The output of the previous step is then replaced by the original co-evolution score. Any residue-residue contacts which have co-evolution and are in contact in each structure are included as a connection dependent on the user input co-evolution and back-backbone angstrom thresholds. The top-ranking co-evolution connections only visible in the multimer are noted and listed as an output, along with their co-evolution score.

Finally in all versions, (except for unguided mode where the output is a 2D contact map), using pymol3d suite, dashes are made between protein-protein connections above a user set threshold in the multimer structure, and the file saved as a pdb, with the option for an angstrom based distance threshold. This shows which amino acids have a high co-evolution score and are in contact dependent on user threshold.  Scripts are available at 
https://colab.research.google.com/drive/1MSSvNTq7KZ4Lr0XTz89vUuK-J3xOTzwS?usp=sharing.


ACKNOWLEDGEMENTS

We would like to thank Sergei Ovchinnikov for his helpful insights into his Github page, and those mentioned in the ‘Beerware license’ of the GREMLIN algorithm. ‘Sergey Ovchinnikov and Peter Koo, as well as Hetu Kamisetty’ who wrote the first GREMLIN algorithm for co-evolution searching. We would also like to thank Phillip Stansfeld for his helpful insights and guidance. This work has been supported by the BBSRC under grant BB/X008533/1.

REFERENCES


1.	Balakrishnan S, Kamisetty H, Carbonell JG, Lee S, Langmead CJ. Learning generative models for protein fold families. Proteins: Structure, Function, and Bioinformatics. 2011 Apr 25;79(4):1061–78. 
2.	Ovchinnikov S. Protein structure determination using evolutionary information. Thesis. 2017; 
3.	Quadir F, Roy RS, Halfmann R, Cheng J. DNCON2_Inter: predicting interchain contacts for homodimeric and homomultimeric protein complexes using multiple sequence alignments of monomers and deep learning. Sci Rep. 2021 Jun 10;11(1):12295. 
4.	Kilian M, Bischofs IB. Co‐evolution at protein–protein interfaces guides inference of stoichiometry of oligomeric protein complexes by de novo structure prediction. Mol Microbiol. 2023 Nov 30;120(5):763–82. 
5.	Steinegger M, Söding J. MMseqs2 enables sensitive protein sequence searching for the analysis of massive data sets. Nat Biotechnol. 2017 Nov 16;35(11):1026–8. 
6.	Mirdita M, Schütze K, Moriwaki Y, Heo L, Ovchinnikov S, Steinegger M. ColabFold: making protein folding accessible to all. Nat Methods. 2022 Jun 30;19(6):679–82. 
7.	Mohammadi T, Van Dam V, Sijbrandi R, Vernet T, Zapun A, Bouhss A, et al. Identification of FtsW as a transporter of lipid-linked cell wall precursors across the membrane. EMBO Journal. 2011; 
8.	Cho H, Wivagg CN, Kapoor M, Barry Z, Rohs PDA, Suh H, et al. Bacterial cell wall biogenesis is mediated by SEDS and PBP polymerase families functioning semi-Autonomously. Nat Microbiol. 2016; 
9.	Nygaard R, Graham CLB, Belcher Dufrisne M, Colburn JD, Pepe J, Hydorn MA, et al. Structural basis of peptidoglycan synthesis by E. coli RodA-PBP2 complex. Nat Commun. 2023 Aug 24;14(1):5151. 
10.	Keon KA, Benlekbir S, Kirsch SH, Müller R, Rubinstein JL. Cryo-EM of the Yeast V O Complex Reveals Distinct Binding Sites for Macrolide V-ATPase Inhibitors. ACS Chem Biol. 2022 Mar 18;17(3):619–28. 
11.	Hopf TA, Morinaga S, Ihara S, Touhara K, Marks DS, Benton R. Amino acid coevolution reveals three-dimensional structure and functional domains of insect odorant receptors. Nat Commun. 2015 Jan 13;6(1):6077. 
12.	Kamisetty H, Ovchinnikov S, Baker D. Assessing the utility of coevolution-based residue–residue contact predictions in a sequence- and structure-rich era. Proceedings of the National Academy of Sciences. 2013 Sep 24;110(39):15674–9. 
13.	Abramson J, Adler J, Dunger J, Evans R, Green T, Pritzel A, et al. Accurate structure prediction of biomolecular interactions with AlphaFold 3. Nature. 2024 Jun 13;630(8016):493–500. 
14.	Jiang Y, Marszalek PE. Atomic force microscopy captures MutS tetramers initiating DNA mismatch repair. EMBO J. 2011 Jul 20;30(14):2881–93. 
15.	Keyamura K, Fujikawa N, Ishida T, Ozaki S, Su’etsugu M, Fujimitsu K, et al. The interaction of DiaA and DnaA regulates the replication cycle in E. coli by directly promoting ATP–DnaA-specific initiation complexes. Genes Dev. 2007 Aug 15;21(16):2083–99. 
16.	Jean NL, Rutherford TJ, Löwe J. FtsK in motion reveals its mechanism for double-stranded DNA translocation. Proceedings of the National Academy of Sciences. 2020 Jun 23;117(25):14202–8. 
17.	Wotherspoon P, Johnston H, Hardy DJ, Holyfield R, Bui S, Ratkevičiūtė G, et al. Structure of the MlaC-MlaD complex reveals molecular basis of periplasmic phospholipid transport. Nat Commun. 2024 Jul 30;15(1):6394. 
18.	Granell M, Namura M, Alvira S, Kanamaru S, Van Raaij M. Crystal Structure of the Carboxy-Terminal Region of the Bacteriophage T4 Proximal Long Tail Fiber Protein Gp34. Viruses. 2017 Jun 30;9(7):168. 
19.	Zeytuni N, Hong C, Flanagan KA, Worrall LJ, Theiltges KA, Vuckovic M, et al. Near-atomic resolution cryoelectron microscopy structure of the 30-fold homooligomeric SpoIIIAG channel essential to spore formation in Bacillus subtilis. Proceedings of the National Academy of Sciences. 2017 Aug 22;114(34). 
 



<img width="451" height="691" alt="image" src="https://github.com/user-attachments/assets/6a764fdb-1123-42b2-99f2-ebe5a96677ef" />

