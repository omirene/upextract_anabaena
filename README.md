# upextract_anabaena
A tool for mass extracting genome sequences a given number of base pairs upstream and downstream of a series of genes' coding sequence. 
Also available as a colab notebook for simple direct use.

This script requires several external items as input that must be provided:
1. The genome dataframe
2. All relevant genomic sequences (chromosome and plasmids, both direct and complementary)
3. A list with the query genes from which to extract the upstream region (a txt file named "querylist.txt", comma-separated and with no headers)
Files for 1. and 2. are included in the repository, along with a sample querylist and one to test out possible errors.

This script requires input on:
- Whether to include the coding region of upstream-encoded genes or not (NOORF option on or off)
- How high upstream and into the cds should the sequence retrieve
These are to be written below in INPUT INFORMATION

LIMITATIONS: this code does not account for genomic circularity. For genes whose upstream sequence would cross into the "end" of the genomic element (such as alr8001 or asr9501), upstream sequence will be limited to up to coordinate 1. The higher the upstream, the more likely this is to happen. An "expected length error" should warn if the upstream sequence goes past the limits of the DNA template in linear form for NOORF="no", but it can't do this for NOORF="yes" since there is no expected length. "Mounted" genes (i.e. those whose ATG is found within the cds of the upstream gene) will add nts in the sequence just before the ATG. Check manually and remove extra nts based on the lengths stated in the error warning.

This code tries to account for all genes and warn for general errors (inclusion of known limited-compatibility cases, incorrect loci, otherwise incorrect retrieved length).

The code will, broadly, follow this scheme:
1. Retrieve the required information about each loci in the query list
2. Determine which genomic configuration it belongs to (direct or complementary strand; upstream gene is on the same strand or not)
3. Locate the coordinates of the desired start and end of the sequence to retrieve
4. Extract the corresponding sequence from the right genomic element and strand

Like this, an "output" will be generated per query, comprising:
Part1: corresponding to the overlap with the upstream-encoded gene, if applicable
Part2: intergenic region
Part3: sequence into the cds

Finally, a resultslist will be updated each cycle to include the newly generated output before iterating.
