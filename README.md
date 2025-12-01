# background
for  paper 


#concatenate files

find ./ -type f -name 'pm0188*coverage' -exec cat {} + > pm0188_coverage.tsv
find ./ -type f -name 'PF11477*coverage' -exec cat {} + > PF11477_coverage.tsv
find ./ -type f -name 'neuS*coverage' -exec cat {} + > neuS_coverage.tsv
find ./ -type f -name 'neuA*coverage' -exec cat {} + > CMP_coverage.tsv
find ./ -type f -name 'PF06002*coverage' -exec cat {} + > PF06002_coverage.tsv
find ./ -type f -name 'lst*coverage' -exec cat {} + > lst_coverage.tsv
find ./ -type f -name 'lic3X*coverage' -exec cat {} + > lic3X_coverage.tsv
find ./ -type f -name 'IPR010866*coverage' -exec cat {} + > IPR010866_coverage.tsv



#create directories
mkdir pm0188_cover_filter
mkdir PF11477_cover_filter
mkdir neuS_cover_filter
mkdir CMP_cover_filter
mkdir PF06002_cover_filter
mkdir lst_cover_filter
mkdir lic3X_cover_filter
mkdir IPR010866_cover_filter


#modify file
#! /bin/bash

for file in ./*NCBI_ID_cover_filter_40.tsv; do
sed -i '1d' $file
awk '{print $2}' $file > "${file}_modified";
done





List of hmm file models

IPR010866_auto.hmm      
lic3X_final_auto.hmm  
lst_final_auto.hmm         
PF06002.hmm  
  neuA_final_mafft_auto.hmm  
  neuS_final_auto.hmm   
   PF11477.hmm  
   pm0188_final_auto.hmm
