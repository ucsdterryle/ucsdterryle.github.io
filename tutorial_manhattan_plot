# Tutorial on Creating a Manhattan Plot

A common plot used in bioinformatics is a Manhattan Plot. It is a elaborate plot which looks like a hybrid of a scatter plot with a histogram. The central idea is to bin the dataset in such a way that allows you to see how data is grouped while also seeing the value of the dependent variable. To generate this plot we will be using the Matplotlib library, Pandas, and Scipy.


For this plot we will be working with a genomic data set involving single nucleotide polymorphism.

(Including background desciption of SNP/z-score/p-value , PrediXcan and other details regarding data set)


Code

                import pandas as pd
                import numpy as np

                import matplotlib.pyplot as plt
                from sklearn import linear_model
                from scipy.special import expit


Lets first open and load the file into a Pandas DataFrame.


                prediXcan = pd.read_csv("SPrediXcan-UKBB-23andme-AUDIT1.meta-updated06132018.csv")
                prediXcan.head(5)


                gene 	gene_name 	zscore 	pvalue 	var_g 	pred_perf_r2 	pred_perf_pval 	n_snps_used 	n_snps_in_cov 	n_snps_in_model 	brain tissue
                0 	ENSG00000035928 	RFC1 	-6.774841 	1.250000e-11 	0.118327 	0.218092 	4.260000e-07 	54 	54 	54 	hemisphere
                1 	ENSG00000238083 	LRRC37A2 	-6.128432 	8.870000e-10 	0.220697 	0.638350 	1.460000e-34 	32 	40 	40 	cerebellum
                2 	ENSG00000262539 	RP11-259G18.3 	-6.064361 	1.320000e-09 	0.228076 	0.611747 	7.250000e-26 	48 	65 	65 	hemisphere
                3 	ENSG00000185294 	SPPL2C 	-5.940040 	2.850000e-09 	0.059510 	0.358462 	5.430000e-15 	26 	38 	38 	cerebellum
                4 	ENSG00000238083 	LRRC37A2 	-5.909682 	3.430000e-09 	0.337754 	0.694215 	9.540000e-34 	36 	49 	49 	hemisphere
                
                
                
                
The data we will be working with are the pvalues and the brain tissue. We want to be able to group all the p-values associated with a particular brain tissue. In addition the p-values are very small values which prompts us to use a logscale for the y-xais plot. 


                #manhattan plot
                fig = plt.figure(figsize=(14, 8)) # Set the figure size
                ax = fig.add_subplot(111)
                colors = ['darkred','red', 'pink', 'green', 'darkgreen', 'teal', 'blue', 'darkblue', 'yellow', 'gold']
                x_labels = []
                x_labels_pos = []
                for num, (name, group) in enumerate(prediXcan_grouped):
                    group.plot(kind='scatter', x='ind', y='logpval',color=colors[num % len(colors)], ax=ax)
                    x_labels.append(name)
                    x_labels_pos.append((group['ind'].iloc[-1] - (group['ind'].iloc[-1] - group['ind'].iloc[0])/2))
                
                ax.set_xticks(x_labels_pos)
                ax.set_xticklabels(x_labels)

                # set axis limits
                ax.set_xlim([0, len(prediXcan)])
                #ax.set_ylim([0, 3.5])

                # x axis label
                ax.set_xlabel('Brain Tissue')

                # show the graph
                plt.show()
                
                
![Manhattan Plot](/pediXcan_manhattanPlot.png)
