# background
for  paper 


#  concatenate files

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


# move files that passed the filter in R into their respectives directories
for file in $(cat ./LST_NCBI_ID_cover_filter_40.tsv_modified); do mv "$file" ./lst_cover_filter/; done
for file in $(cat ./IPR01086_NCBI_ID_cover_filter_40.tsv_modified); do mv "$file" ./IPR010866_cover_filter/; done
for file in $(cat ./Lic3_NCBI_ID_cover_filter_40.tsv_modified); do mv "$file" ./lic3X_cover_filter/; done
for file in $(cat ./neuS_thais_NCBI_ID_cover_filter_40.tsv_modified); do mv "$file" ./neuS_cover_filter/; done
for file in $(cat ./PF06002_NCBI_ID_cover_filter_40.tsv_modified); do mv "$file" ./PF06002_cover_filter/; done
for file in $(cat ./PF11477_NCBI_ID_cover_filter_40.tsv_modified); do mv "$file" ./PF11477_cover_filter/; done
for file in $(cat ./pm1088_NCBI_ID_cover_filter_40.tsv_modified); do mv "$file" ./pm0188_cover_filter/; done

#process files at once: cat all files, remove "#" in the output and let tab formatted

for d in ./*_filter; do
cat "$d"/*output.tsv > "${d}_all.tsv"
sed -i '/#/d'  "${d}_all.tsv"
sed -i 's/ \{1,\}/\t/g' "${d}_all.tsv" 
done

# Go to hmm_process_final_paper.ypnb script to handle hmmer output to filter by e-value and bit-score. Also get sequences ID for Interpro analysis

#Now move proteomes files that have a previous sialylation before Interpro verification
#first append "_protein.faa" to match coordinates of file. Go to proteins folder

cd proteins
sed  's/$/_protein.faa/g' completesialylation_GCF_ID.tsv > completesialylation_GCF_ID_files.tsv

sed -i '1d' completesialylation_GCF_ID_files.tsv #remove header
mkdir proteins_sialylation #where proteins files are going to be reallocated
for file in $(cat ./completesialylation_GCF_ID_files.tsv); do mv "$file" ./proteins_sialylation/; done
ls proteins_sialylation | wc -l # output with 3,636 files

# retrieve sequences for Interpro analysis
mv *for_Interpro.tsv ./proteins_sialylation/ #move Proteins IDs files to specific folder
cd ./proteins_sialylation/ 
sed -i '1d' *for_Interpro.tsv

#modify files fasta header to match exactly to the pattern search
for f in ./*.faa; do
    awk '{if(substr($0,1,1)==">") print $1; else print}' "$f" > "$f.tmp" && mv "$f.tmp" "$f"
done

bash retrieve_sequences_for_interpro.sh #run to retrieve sequences

#check if everything is alright
wc -l pm0188_ID_sequence_for_Interpro.tsv #318 lines
grep -c ">" pm0188_ID_sequence_for_Interpro.tsv_retrieved #318 fasta headers
wc -l  neuS_ID_sequence_NCBI_and_PFAM_for_Interpro.tsv #2,527 lines
grep -c ">" neuS_ID_sequence_NCBI_and_PFAM_for_Interpro.tsv #2,527 fasta headers
wc -l Lic3_ID_sequence_for_Interpro.tsv #435
grep -c ">" Lic3_ID_sequence_for_Interpro.tsv_retrieved #435
wc -l Lst_ID_sequence_for_Interpro.tsv #1453
grep -c ">" Lst_ID_sequence_for_Interpro.tsv_retrieved #1453
wc -l CMP_ID_sequence_for_Interpro.tsv #3,636
grep -c ">" CMP_ID_sequence_for_Interpro_retrieved #3,636

# Run Intersproscan
bash interpro_analysis_final.sh



# take header of uhgp-faa file
sed -i 's/ .*//' uhgp-100.faa



#Download interproScan version 5.76-107.0. See the instructions to installation [here: https://interproscan-docs.readthedocs.io/en/v5/HowToDownload.html].


List of hmm file models

IPR010866_auto.hmm      
lic3X_final_auto.hmm  
lst_final_auto.hmm         
PF06002.hmm  
  neuA_final_mafft_auto.hmm  
  neuS_final_auto.hmm   
   PF11477.hmm  
   pm0188_final_auto.hmm
