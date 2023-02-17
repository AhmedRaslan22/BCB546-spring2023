# UNIX Assignment#1

```
$ mkdir Unix_Assigment1  

$ cp snp_position.txt fang_et_al_genotypes.txt /home/raslan/BCB546_Spring2023/assignments/UNIX_Assignment/Unix_Assigment1/
```
## 1. Data inspection for both files




'''Inspect the number of lines, words, characters, size, and columns'''
```
$ wc snp_position.txt
$ du -h snp_position.txt
$ awk -F "\t" '{print NF; exit}' snp_position.txt




$ wc fang_et_al_genotypes.txt
$ du -h fang_et_al_genotypes.txt
$ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
```



## 2. Data processing for both files

'''' cutting columns 1,3, and 4 columns that have SNP_ID, chromosome, and position. '''
```
$ cut -f1,3,4 snp_poistion_txt > cut_SNP-position.txt 
```







## maize


''' extracting lines of the fang_et_al_genotype.txt file that have the matches Group/ ZMMIL/ ZMMLR/ZMMMR'''
```
$ grep  "Group\|ZMMIL\|ZMMLR\|ZMMMR" fang_et_al_genotypes.txt > maize_fang.txt 
```

''' Transposing the maize _fang.txt file '''

```
$ awk -f transpose.awk maize_fang.txt > transposed_maize.txt
```

'''Displaying the first ten columns of " transposed_maize.txt "'''
```
$ cut -f1-10 transposed_maize.txt
```
''' Taking off the first two lines of " transposed_maize.txt " ''''
```
$ tail -n +3 transposed_maize.txt > 2headsoff_transposed_maize.txt
```
''' joining " cut_SNP_position.txt" and "2headsoff_transposed_maize.txt", ignoring their header '''
 ```
$ join -1 1 -2 1 -t $'\t'  --header cut_SNP_position.txt 2headsoff_transposed_maize.txt > joined_maize_snp.txt
```
''' Extracting the header and lines containing unknowns and multiples from "joined_maize_snp.txt" into "maize_unknown.txt" and " maize_multiple.txt"'''

```
$ grep -E "(SNP_ID|unknown)" joined_maize_snp.txt > maize_unknown.txt 

$ grep -E "(SNP_ID|multiple)" joined_maize_snp.txt > maize_multiple.txt 
```



'''sorting "joined_maize_snp_txt" based on snp positions in increasing order not including the first line in the sort while adding it to the output folder "K3_Sorted_joined_maize_snp.txt" '''
```
$ head -n1 joined_maize_snp.txt > K3_Sorted_joined_maize_snp.txt && tail -n +2 joined_maize_snp.txt | sort -k3,3V >> K3_Sorted_joined_maize_snp.txt 
```
'''extracting chromosomes (1-10)'''
```
$ awk -F "\t" '{print NF; exit}' K3_Sorted_joined_maize_snp.txt                 1576

$ head -n1 K3_Sorted_joined_maize_snp.txt > 1ch_maize.txt | awk '{if(NF==1576 && $2=="1") print $0}' K3_Sorted_joined_maize_snp.txt >> 1ch_maize.txt
```
''' repeat the previous command to extract all the ten columns while exchanging the corresponding numbers (1-10)'''

''' Create 2 separate up/down directories and duplicate all the ten extracted chromosomes files there '''

``` 
$ mkdir "maize_up" 
$ mkdir "maize_down"
```

''' all the chromosomes  (1-10)ch_maize.txt in the up directory are arranged in increasing order with missing data as "?" '''

''' for chromosomes in the down directory, run this command to replace the missing data of all files with "-" at once''' 
```
$ sed -i 's/?/-/g' *.txt
```
''' reverse sort based on snp position, not including the header in the sort while adding it to the output file'''
```
$ head -n1 1ch_maize.txt > down_1ch_maize.txt ; tail -n +2 1ch_maize.txt | sort -k3,3nr >> down_1ch_maize.txt 
```
'''repeat the same command for all chromosomes.''






## teosinte 

''' Same processing as maize ''''
```
$ grep  "Group\|ZMPBA\|ZMPIL\|ZMPJA" fang_et_al_genotypes.txt > teosinte_fang.txt

$ awk -f transpose.awk teosinte_fang.txt > transposed_teosinte.txt

$ cut -f1-10 transposed_teosinte.txt

$ tail -n +3 transposed_teosinte.txt > 2headsoff_transposed_teosinte.txt

$ head -n1 2headsoff_transposed_teosinte.txt

$ join -1 1 -2 1 -t $'\t'  --header cut_SNP_position.txt 2headsoff_transposed_teosinte.txt > joined_teosinte_snp.txt

$ grep -E "(SNP_ID|unknown)" joined_teosinte_snp.txt > teosinte_unknown.txt

$ grep -E "(SNP_ID|multiple)" joined_teosinte_snp.txt > teosinte_multiple.txt

$ head -n1 joined_teosinte_snp.txt > K3_Sorted_joined_tesosinte_snp.txt && tail -n +2 joined_teosinte_snp.txt | sort -k3,3V >> K3_Sorted_joined_teosinte_snp.txt

$ awk -F "\t" '{print NF; exit}' K3_Sorted_joined_teosinte_snp.txt 978

$ head -n1 K3_Sorted_joined_teosinte_snp.txt > 1ch_teosinte.txt | awk '{if(NF==1576 && $2=="1") print $0}' K3_Sorted_joined_teosinte_snp.txt >> 1ch_teosinte.txt

$ mkdir "up_teosinte"

$ mkdir "down_teosinte" 

$ sed -i 's/?/-/g' *.txt

$ head -n1 1ch_teosinte.txt > down_1ch_teosinte.txt ; tail -n +2 1ch_teosinte.txt | sort -k3,3nr >> down_1ch_teosinte.txt
```





