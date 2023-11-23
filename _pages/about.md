---
permalink: /
title: "Novel quantifiable metrics for the identification of non-systemic small drugs"
excerpt: "About me"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

Miles Benardout, Stephen P. Wren, Adam Le Greseley, Amr ElShaer

Kingston University

Introduction
======
The drug design process requires a medicinal chemist to not only consider the properites of a compound that alter the therpeautic activity of a compound, but also the pharmacological properties of how a compound behaves in the body, commonly known as the properties affecting Absorption, Distribution, Metabolism, and Excretion (ADME). These ADME characteristic define the bioavailbility of a given drug.

Typically, the majority drugs going to market are required to be orally bioavailable. This is often predicted in the early stage of the design design process by well-known 'rule of thumb' approaches, such as Lipinski's rule of 5 criteria, and later filters such as the Veber, Ghose, and Mugge filters. Although they are useful methods for the identification of orally bioavaible compounds, over-reliance on these filters has lead to an over-restriction of chemical space for small molecules, and although fairly accurate for identifying orally bioavaible compounds, they are notably weaker at accurately identifying compounds with poor oral bioavaibility.

Our goal was to develop a quantifiable metric to identify pre-exsisting compounds with a high likelihood of poor oral bioavailbility, in order to create a library of small molecules with high gut residence. Please see below a diagram describing our workflow for the project as a whole to identify potential non-systemic fructose scavengers

![workflow](/images/workflow_ymf.png)

Fructose Malabsorption
======
The goal of our discovery of non-systemic drugs is to identify suitable candidates for the treatment of Fructose Malabsorption, a condition related to the body's inability to absorb an excess of fructose in the small intestine, leading to symptoms commonly associated with Irritable Bowel Syndrome. It is known that the phenylboronic acid functional group is able to bind to fructose, and we aim to develop a library of non-sytemic fructose scavengers for the treatment of fructose malabsorption. Current treatments for fructose malabsorption are typically preventative, and primarily consist of dietary interventions; however, there is room for novel medicinal intervention in the treatment of fructose malabsorption.

Data mining
======
In order to create our library of candidate fructose scavengers, we focused on pre-existing phenylboronic acids found in the literature, as listed by freely available compound libraries, such as ChEMBL, PubChem, and Zinc. As a intial proof of concept for our scoring function, compound data will be exclusively derived from PubChem. A substructure search for phenylboronic acid (SMILES: OB(O)C1=CC=CC=C1 ) yielded at total of 220284 compounds for initial analysis
since we are aiming to identify small molecules that we predict to have a high gut residence, we limited the maximum molecular weight to 700, resulting in a total of 157,514 compounds.

Initial compound filtering and processing
======
As with any data set, the data must be pre-processed prior to analysis, to ensure the quality of analysis ("garbage in, garbage out"). In our case, the dataset containing, in addition to the phenyl boronic acid compounds we want to analyse, a number of benzoxoboroxoles, and pinacol-protected compounds. After filtering out these unwanted compounds, we were left with **48717** compounds for evaulation.

Selection and calculation of Physiochemical descriptors
======
In order to predict non-druglikess in the longlist of compounds, we needed a consistent method of calculating the physiochemical (ADME) descriptors for each compound. Using RDKit, a free and open source cheminformatics library for the Python programming language, we were able to calculate descriptors in bulk for all compounds. Although we aim to design a metric that is inherently modular in nautre, enabling the incorporation of any additional descriptors, we chose to select descriptors that we felt confident in describing their effect on oral bioavailability. 

These include:
- Lipophilicity (logP)
- Molar Refractivity (MR)
- Topological Polar Surface Area (TPSA)
- Molecular weight (MW)
- Number of rotatable bonds (RotB)
- Number of Hydrogen bond donors (HBD)
- Number of Hydrogen bond acceptors (HBA)
- Fraction of Carbon atoms that are sp3 hybridised (FSP3)

Once calculated, it was important to consider that all these descriptors have different scales, and in order to equally weight each descriptors for relative comparison, each value must be normalised in a way that gives greater value to raw values that may indicate poor druglikeness. For MR, TPSA, MW, RotB, HBD and HBA, higher values will indicate poor oral bioavailability; for FSP3, lower values indicate poor oral bioavailability, and for logP, increasing distance from the mean logP (i.e. extreme values higher or lower than the mean) will indicate poor oral bioavailability. By taking into account these criteria, and the mean and standard deviation for each descriptor, normalised values for each descriptor were obtained.

The scoring function
======
The scoring function used to identify non-sytemic drugs is shown below

<img src="/images/scoring_formula.png" alt="equation" width="250" height="250"/>
Where:
  *S* = score (higher = less druglike)
  *Za* = normalised descriptor value
  *W* = weighting (in the case of this proof-of-conception, always equals one)
