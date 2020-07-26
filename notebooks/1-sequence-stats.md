# Julia Code for calculating mean, std of sequence lengths and gc content of genomes

After separating the contents of the sequences fasta file into headers and sequences,
we will find the length of each sequence and use this to calculate the mean, std of 
the sequence lengths. The mean, std gc content in each sequence will also be calculated.

virus genomes were dowloaded from NCBI
(see `data/data.md` for more details)

```julia
using SequenceSummary
fasta_path = normpath(joinpath(@__DIR__, "..", "CoV2", "data", "SARS-CoV-2+MERS.fasta"))
parsed_seq = parse_fasta(fasta_path); 
seq_lengths = [] #vector to store lengths of each sequence
seq = parsed_seq[2]; #assign sequences to seq
seq_gc = []
for n in 1:length(parsed_seq[2])
    push!(seq_lengths, length(seq[n])) #add length of each sequence to seq_lengths
    push!(seq_gc, gc_content(seq[n])) #add gc ratio of each sequence to seq_gc
end
using Statistics
#sequence length stats
mean_seq_length = mean(seq_lengths)
std_seq_length = std(seq_lengths)
#sequence gc content stats
mean_seq_gc = mean(seq_gc)
std_seq_gc = std(seq_gc)
#finding the max and min sequences in fasta file
min_seqlen = minimum(seq_lengths)
max_seqlen = maximum(seq_lengths)

#find the unique kmers in each sequence
kmer_storage = Dict()
for i in 1:length(seq)
    kmer_storage[i] = kmer_match(seq[i], 8)
end

#find the distance between each sequence kmer set
for i in 1:length(seq)
    for n in 1:length(seq)
        dist_metric = kmer_dist(kmer_storage[i], kmer_storage[n])
        if dist_metric 



```
