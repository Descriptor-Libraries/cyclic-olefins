# Library Details

## Molecule Identification
869 literature reported cyclic olefin G3-ROMP compatible monomer compounds were computed and combined into one uniform library.

## Conformational Searching & Conformer Selection
Candidate monomers were encoded as SMILES strings and converted into SDF and XYZ using OpenBabel, then minimized with Universal Force Field (UFF). If errors arose due to the size of the molecule, RDKit was used, then minimized with either MMFF (Merck Molecular Force Field) or UFF. 

3D geometries were subjected to conformational searches using Conformer-Rotamer Ensemble Sampling Tool (CREST, version 2.12, from Grimme and coworkers) with the GFN-FF (xTB version 6.4.0). An energy window of 5 kcal mol–1 and subsequent PCA/k-Means clustering were applied to refine the conformer ensemble to 50 or fewer conformers per structure, representing energetically relevant and conformationally diverse monomer flexibility.

## DFT Computations
The conformer ensembles were subjected to DFT geometry optimization and energy correction calculations using Gaussian (Gaussian16, Revision C.01) at the ωB97X-D/def2-TZVP//ωB97X-D/def2-SVP level of theory in gas phase with standard temperature and pressure (298.15 K, 1 atm). The ωB97X-D functional includes empirical dispersion correction. Determination of the natural bond orbitals (NBO) was performed using the NBO program (version 7.0). NMR shifts were computed using the gauge-invariant atomic orbitals (GIAO) method. For conformers where the optimization calculation did not terminate normally or the frequency calculations resulted in an imaginary frequency more negative than -100 cm-1, the conformer was resubmitted specifying tight convergence criteria (using Gaussian keyword opt=tight) and the calculation of force constants (using Gaussian keyword opt=calcfc). Several attempts were made; however, if a job could not terminate normally or an optimized structure devoid of any negative frequencies could not be obtained for a given conformer, that conformer was removed from the ensemble for descriptor collection. An energy window cutoff of 5 kcal mol–1 was repeated prior to feature collection, using DFT-computed energies. 

## Property Collection 
![Atom numbering of the cyclic olefin core](/cyclic-olefins/content/cyclicolefin_atommap.png)
Atom numbering and property collection was automated to support high-throughput library construction and analysis. The following steps and supporting code can be found in https://github.com/samanthasloane/CyclicOlefin_ReactivityRatio_Project.

1. The SMARTS pattern ‘CC=C[#6,#16]’ was used to identify atom numbers and elements appearing in or adjacent to olefins. 
2. Skipped all compounds containing multiple olefins except for those that were evaluated experimentally, T22, E1, and E4. For those compounds, atom mapping was done manually to ensure properties were collected on the correct olefin.
3. Olefinic positions were distinguished electronically by comparing Boltzmann weighted average Hirshfeld atomic charges across all conformers, where C1 is designated as the position with the more negative partial charge, or more electron rich.
4. A series of electronic, steric, and geometric properties were computed and/or collected for each conformer using GetProperties and MORFEUS packages.
5. A quasi-harmonic thermochemical correction was applied to the energies associated with each conformer using GoodVibes. Collected properties were weighted across conformational ensembles. The resulting feature sets included feature maximum, minimum, mean, range, standard deviations, as well as a features from the lowest energy conformer and an approximation to a Boltzmann-weighted average. 
6. Additionally, symmetry-related maximum, minimum, averages, or differences of allylic and olefinic positions were computed between related positions. 
7. The descriptor set was curated to improve interpretability, including the removal of maximum, minimum, standard deviation, and range statistics for frontier orbital properties.

The dynamic range of properties across the conformational ensemble, and symmetry related positions on olefinic or allylic carbons, of a single cyclic olefin monomer was summarized by using the following:

| Descriptor              | Definition                                                                                                                                      |
| ----------------------- | ------------------------------------------------------------------------------------------------                                                |
| Boltz                   | Boltzmann-weighted average of a property from all the conformers in an ensemble (T = 298.15 K)                                                  |
| max                     | highest value of a property given by any conformer in an ensemble                                                                               |
| min                     | lowest value of a property given by any conformer in an ensemble                                                                                |
| low_E                   | value of a property from the lowest energy conformer in the ensemble                                                                            |
| range                   | absolute value of the max - min property value within an ensemble                                                                               |
| Boltz_stdev             | Standard deviation of the Boltzmann-weighted average of a property from all the conformers in an ensemble                                       |
| Boltz_maxsym            | The maximum of the two symmetry related positions after Boltzmann-weighted averaging of the property from all the conformers in an ensemble     |
| Boltz_minsym            | The minimum of the two symmetry related positions after Boltzmann-weighted averaging of a property from all the conformers in an ensemble       |
| Boltz_averagesym        | The average of the two symmetry related positions after Boltzmann-weighted averaging of a property from all the conformers in an ensemble       |
| Boltz_deltasym          | The difference of the two symmetry related positions after the Boltzmann-weighted averaging of a property from all the conformers in an ensemble|


## Citation
Ko, K.; Sloane, S.; Hart, S.; Zhang,  S.; Sigman, M. S.; Johnson, J. A. "A Data Science Approach Toward the Sequence-Aware Design of Ring-Opening Metathesis Polymerization (ROMP)-Derived Copolymers." *Manuscript in Preparation.*