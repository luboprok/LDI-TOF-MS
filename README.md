# LDI-TOF-MS
The presented R workflow has been developed for the analysis of mass spectra of inorganic materials. The main goal is to create a script that will be able to automatically identify overlapping complex isotope distributions in mass spectra. This will primarily be done by estimating possible stoichiometries based on the assumed elemental composition of the material and possible impurities. Generate model distributions of the predicted stoichiometries. Calculate the percentage of each cluster that fits the experimental data and the final fit to these data. 
In addition, the workflow presented includes a number of additional features such as batch processing of mass spectra, pre-processing and alignment of spectra. Functions for calculating Kendrick mass, comparing the number of identified signals from experimental data with individual models, and the ability to detect isobaric interferences in ICP-MS.
# DATA
Data from an LDI-TOF MS analysis of an elemental mixture of gallium and selenium were used as example data to demonstrate the proposed R script. The dataset contains 5 exported mass spectra in text format (.txt) together with metadata. In addition, the exported mass spectra are used in Excel format (.xlsx). The data were acquired on an Axima Resonance quadrupole ion trap mass spectrometer (Kratos Analytical, Manchester, UK).
# STRUCTURE
The code has been divided into three subsets in order to maintain clarity:
The initial subset focuses on a comprehensive analysis of the mass spectrum, including preliminary processing steps such as transformations, baseline correction, and normali-zation. It also involves identifying recurring patterns in the data, such as Kendrick mass. To facilitate this, the mass spectrum is segmented using automatic clustering based on signal-to-noise ratio or predefined thresholds. Additionally, the script integrates companion functions to detect isobaric contamination in mass spectra, a common issue in ICP-MS (May, Wiedmeyer, 1998; Rodríguez-Castrillón, 2009).
In the second part, the code performs a detailed analysis of a specific region of the mass spectrum. For the selected region, an in-depth isotopic distribution analysis is con-ducted, including the calculation of the theoretical stoichi-ometry of potential clusters, their relative abundance, and the degree of fit based on the similarity of the overall iso-topic distribution. Furthermore, a comparison with theoreti-cal models is carried out by assessing the deviations be-tween the m/z positions of experimental and theoretical data. Additionally, the code includes a function for calcu-lating the monoisotopic mass, enhancing the accuracy of isotopic characterization.
The last section focused on the analysis of spectra containing isotopically low-abundant elements and monoi-sotopic elements, where regularly repeating series were observed, characterized by a gradual increase in the num-ber of atoms of a specific element.
# DESCRIPTION OF THE UTILIZED LIBRARY
Libraries MALDIquant (Gibb, Strimmer 2012) and MALDIrppa are comprehensive packages for pre-processing mass spectra. This comprehensive libraries facilitate quality control, implements a variance-stabilising transformation with smoothing, and eliminates chemical background through baseline correction. Furthermore, it provides calibration for both intensity values (normalisation) and m/z values (alignment). Some pre-processing and diagnostics functions are also available in MassSpecWavelet, MsCoreUtils (Rainer et al. 2022), prospectr and PROcess libraries. MALDIquant and MALDIrppa libraries also facilitate peak detection, and includes options for post-processing filtering. Peaks identification functions are also involved in the R libraries pracma and scorepeak.

There is a significant scarcity of approaches for modeling, selecting, and fitting the isotopic distributions of inorganic clusters. The calculation of cluster stoichiometry can be performed using the rcdk library (Guha 2007), as the Rdisop and enviGCMS libraries are primarily designed for use with organic molecules and therefore not suitable for this purpose. 

In order to model isotopic patterns, the R library enviPat (Loos et al. 2015). is the most applicable, as the alternative libraries BRAIN and OrgMassSpecR are only applicable to organic molecules.

The fitting of a linear combination of individual cluster spectra to a measured spectrum is achieved through nonlinear regression via the Port algorithm (function nls in the basic statistics library), which is subsequently followed by the calculation of a mixed isotopic pattern.

Kendrick mass and Kendrick mass defect are commonly used for identification of homologous compounds differing only by a number of base units in analysis of organic compounds and their mixtures (Hughey et al. 2001; Fouquet 2019; Merel 2023) or organic derivatives (e.g. Shah et al. 2007). However, it can be also employed for analysis inorganic compounds of monoisotopic elements (e.g. Prokeš et al. 2014) or oxidized species (e.g. Allen et al. 1996; Ayers et al. 2014; Huang et al. 2023) in the future. With R software, they can be calculated MsCoreUtils or enviGCMS. Average (atomic/molecular) mass calculation is included in the libraries biogas, CHNOSZ, marelac, MSbox, PeriodicTable, only for organic molecules also BRAIN and OrgMassSpec libraries are available. For the exact mass calculation related functions are included in enviGCMS and InterpretMSSpectrum libraries, functions calculation of monoisotopic mass are available only for organic molecules in BRAIN, IDSL.UFA (Fakouri Baygi et al. 2022) and OrgMassSpec libraries.

For parsing of molecular formulas and subsequent chemical elements counting CHNOSZ and MetaboCoreUtils libraries.

In addition to LDI-TOF MS, the functions can be easily adapted and used to analyse spectra obtained by other mass spectrometry methods, e.g. spectra of organometallic compounds from GC-MS or organic derivatives (e.g. Duffield et al. 1968; Meija et al. 2005; Cody, Fouquet 2019), or to model isobaric inter-ferences for ICP-MS (e.g. May, Wiedmeyer 1998; Rodríguez-Castrillón 2009).

References:
1)  Allen, T. M., Bezabeh, D. Z., Smith, C. H., McCauley, E. M., Jones, A. D., Chang, D. P. Y., Kennedy, I. M., Kelly, P. B. 1996. Speciation of arsenic oxides using laser desorption/ionization time-of-flight mass spectrometry. Analytical Chemistry, 68 (22), 4052-4059. DOI: 10.1021/ac960359z

2)  Ayers, T. M., Akin, S. T., Dibble, C. J., Duncan, M. A. 2014. Laser desorption time-of-flight mass spectrometry of inorganic nanoclusters: An experiment for physical chemistry or advanced instrumentation laboratories. Journal of Chemical Education, 91 (2), 291-296. DOI: 10.1021/ed4003942
3)  Cody, R., Fouquet, T. 2019. Elemental composition determinations using the abundant isotope. Journal of the American Society for Mass Spectrometry, 30 (7), 1321-1324. DOI: 10.1007/s13361-019-02203-9
4)  Duffield, A. M., Djerassi, C., Mazerolles, P., Dubac, J., Manuel, G. 1968. Mass spectrometry in structural and stereochemical problems CLII. Electron impact promoted fragmentation of some substituted germacyclopentanes and germacyclopentenes. Journal of Organometallic Chemistry, 12 (1), 123-132. DOI: 10.1016/S0022-328X(00)90905-7
5)  Fakouri Baygi, S., Banerjee, S. K., Chakraborty, P., Kumar, Y., Kumar Barupal, D. 2022. IDSL.UFA assigns high-confidence molecular formula annotations for untargeted LC/HRMS data sets in metabolomics and exposomics. Analytical Chemistry, 94 (39), 13315−13322. DOI: 10.1021/acs.analchem.2c00563
6)  Fouquet, T. N.J. 2019. The Kendrick analysis for polymer mass spectrometry. Journal of Mass Spectrometry, 54 (12), 933-947. DOI: 10.1002/jms.4480
Gatto, L., Rainer, J., Vicini, A., Salzer, L., Stanstrup, J., Badia, J. M., Neumann, S., Stravs, M. A., Verri Hernandes, V., Gibb, S., & Witting, M. 2022. A modular and expandable ecosystem for metabolomics data annotation in R. Metabolites, 12(2), 173. DOI: 10.3390/metabo12020173

7)  Gibb, S., Strimmer, K. 2012. MALDIquant: a versatile R package for the analysis of mass spectrometry data. Bioinformatics, 28 (17), 2270–2271. DOI: 10.1093/bioinformatics/bts447.
8)  Guha, R. 2007. Chemical informatics functionality in R. Journal of Statistical Software, 18 (5), 1-16. DOI: 10.18637/jss.v018.i05
Huang, X., Li, Y., Qu, G., Yu, X.-F., Cao, D., Liu, Q., Jiang, G. 2023. Molecular-level degradation pathways of black phosphorus revealed by mass spectrometry fingerprinting. Chemical Science, 14, 6669. DOI: 10.1039/D2SC06297F
9)  Hughey, C. A., Hendrickson, C. L., Rodgers, R. P., Marshall, A. G., Qian, K. 2001. Kendrick mass defect spectrum: A compact visual analysis for ultrahigh-resolution broadband mass spectra. Analytical Chemistry, 73 (19), 4676–4681. DOI: 10.1021/ac010560w
10)  Loos, M., Gerber, C., Corona, F., Hollender, J., Singer, H. 2015. Accelerated isotope fine structure calculation using pruned transition trees. Analytical Chemistry, 87 (11), 5457-5848. DOI: 10.1021/acs.analchem.5b00941
11)  May, T. W., Wiedmeyer, R. H. 1998. A table of polyatomic interferences in ICP-MS. Atomic Spectroscopy, 19 (5), 150-155. DOI: 10.46770/AS.1998.05.002
Meija, J., Caruso, J. A. 2004. Deconvolution of isobaric interferences in mass spectra. Journal of the American Society for Mass Spectrometry, 15 (5), 654-658. DOI: 10.1016/j.jasms.2003.12.016
12)  Merel, S. 2023. Critical assessment of the Kendrick mass defect analysis as an innovative approach to process high resolution mass spectrometry data for environmental applications. Chemosphere, 313, 137443. DOI: 10.1016/j.chemosphere.2022.137443
13)  Prokeš, L., Peña Méndez, E. M., Conde, J. E., Reddy Panyala, N., Alberti, M., Havel, J. 2014. Laser ablation synthesis of new gold arsenides using nano-gold and arsenic as precursors. Laser desorption ionisation time-of-flight mass spectrometry and spectrophotometry. Rapid Communications in Mass Spectrometry, 28 (6), 577-86. DOI: 10.1002/rcm.6815
14)  Rodríguez-Castrillón, J. Á., Moldovan, M., García Alonso, J. I. 2009. Internal correction of hafnium oxide spectral interferences and mass bias in the determination of platinum in environmental samples using isotope dilution analysis. Analytical and Bioanalytical Chemistry, 394, 351-362. DOI: 10.1007/s00216-009-2681-4
