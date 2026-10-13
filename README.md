# Analysis-of-Site-Level-Discrimination-in-High-Frequency-NILM-Data
Analysis of Site-Level Discrimination in High-Frequency NILM Data: Acquisition Kit and Load Composition in WHITED v1.1

**Abstract** — High frequency non intrusive load monitoring (HF NILM) uses electrical waveform features to characterize loads, but datasets collected at different sites may also differ in acquisition hardware, grid conditions, recording sessions, and appliance composition. This study audits WHITED v1.1 and investigates whether site level discrimination can be separated from these factors. After removing duplicate files and excluding an undocumented site absent from the official dataset count, we analyzed 1,259 observations from
126 appliances across eight measurement sites and two acquisition kits, using 12 current and voltage waveform descriptors. The site level Silhouette coefficient was −0.0570, above its permutation baseline (mean −0.198) but indicating no well separated global clusters; the kit grouping yielded 0.2430. A Random Forest achieved 72.1% accuracy (71.5% balanced) from the waveform descriptors, increasing to 78.3% (78.6%) when kit information was added. Across ten partitions, the kit improved accuracy by 4.5 ± 1.2 percentage points, whereas no consistent gain occurred within the three sites shared by both kits. Classification remained above the majority baseline when entire appliance types were held out, but site, kit, recording period, and load composition remained confounded. Voltage only descriptors provided little site discrimination performance, and the only 60 Hz site could not be separated from its grid condition. The results demonstrate site associated predictive information in HF NILM features, but do not
establish an independent geographic electrical signature.

**Keywords** —NILM; WHITED; machine learning; Random Forest; PCA; Silhouette coefficient; domain heterogeneity 

**Conclusions and Future Work**
This study audited WHITED v1.1 and evaluated site level discrimination using 1,259 observations from 126 appliances across eight measurement sites. The waveform descriptors enabled site classification, but the negative global Silhouette coefficient indicated no
well separated site clusters. Acquisition kit grouping showed stronger structure, and adding kit improved
global accuracy by 4.5 percentage points on average across ten partitions; this gain was not observed within
the three sites shared by both kits. Discrimination remained above the majority baseline
when entire appliance types were held out, but site, kit, recording period, and appliance composition remained confounded. The grid analysis could not isolate an independent contribution of mains frequency, and uncalibrated voltage amplitudes limited physical interpretation. Thus, the results support site associated predictive information, not an independent geographic
electrical signature.
Future work should use balanced measurements across sites and kits, record calibrated electrical quantities and grid frequency, and measure the same appliances under multiple acquisition conditions. We do not evaluate the effect of these confounders on appliance recognition; this is a necessary next step. These data would enable more reliable tests of cross site generalization and the subsequent evaluation of domain adaptation methods.
