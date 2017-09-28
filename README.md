# Geuvadisdata

import pandas as pd
import matplotlib.pyplot as plt 

import numpy as np 


xls = pd.ExcelFile("data.xls") ## reads in excel file 
#xls.sheet_names
df = xls.parse(0)


#df = xls.parse("Sheet1") ## in pandas, data frame is a common data structure for parsing through excel "type"file, here, I'm dealing with rows and columns
width = 252
height = 53934

#Matrix = [[0 for x in range(height)] for y in range(width)] ## create a matrix by looping through height and width x,y

##applying interquartile calculations 
q25 = [0 for x in range(height)]
q75 = [0 for x in range(height)]


main_matrix = df.iloc[0:, 4:].as_matrix() ##chopping reasons not 1 for iloc
print(len(main_matrix))
print(len(main_matrix[0]))
for i in range(0,53934): ##loop through the columns
	row = main_matrix[i]
	#row = df.iloc[i+1]
	#row = row[4:]
	sor = sorted(row)
	#col =  df.icol(i) ##setting column to a data frame (df is typically used in pandas when selcting columns and rows  without directly stating columns and rows)
	#sor = sorted(col);
	#sor = [1,2,3,4,5,6,7,8,9]
	#q25th,q75th = np.percentile(sor, [25,75])
	q25th = np.percentile(sor,25)
	q75th = np.percentile(sor,75)
	#print('sor')
	q25[i] = q25th
	q75[i] = q75th
	
Matrix = [[0 for x in range(width)] for y in range(height)] ## create a matri
for i in range(0, height): ##looping through each person [i]
    for j in range(0,width): ## looping through each gene [j]
        value = main_matrix[i][j]                     #value = df[i+4, j+1] ## 
        if value < q25[i] or value > q75[i]:
            Matrix[i][j] = 1
## After this loop runs, Matrix[i,j]=1 means the j'th gene of person i is an outlier 
print Matrix 


gene_data = main_matrix[[9,19,72,84]][:]
#gene_data = main_matrix[20][:]
#gene_data = main_matrix[9][:]
#gene_data = main_matrix[:][0]
plt.figure()
plt.boxplot(gene_data.transpose())
plt.show()
