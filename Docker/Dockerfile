#This docker file will create a docker container for the CommonKmers method

FROM cami/profiling

MAINTAINER David Koslicki version: 0.1


#Install Julia, force zero exit code for apt-get update due to outdated package called "wheezy" (who knows what that is!)
RUN apt-get -y udate && \
 apt-get install -y software-properties-common python-software-properties  && \ 
 add-apt-repository -y ppa:staticfloat/juliareleases && \ 
 add-apt-repository -y ppa:staticfloat/julia-deps
RUN apt-get -y update || echo "ok" 
RUN apt-get install -y julia && \ 
 apt-get install -y hdf5-tools && \
 julia -e 'Pkg.add("HDF5"); Pkg.add("ArgParse");'

#Get the CommonKmers code, Install query per sequence and count_in_file
RUN apt-get install -y git && \
 git clone https://github.com/dkoslicki/CAMIARKQuikr.git

#Install DNAUtils
RUN apt-get install -y build-essential && \
 git clone https://github.com/EESI/dna-utils.git && \
 cd dna-utils/ && \
 make && \
 cp kmer_total_count /usr/local/bin && \
 cp kmer_counts_per_sequence /usr/local/bin
 
#Install gzip
RUN apt-get install -y gzip

#Install HMMER3
RUN apt-get install -y wget && \
 wget ftp://selab.janelia.org/pub/software/hmmer3/3.1b2/hmmer-3.1b2.tar.gz && \
 tar xf hmmer-3.1b2.tar.gz && \
 cd hmmer-3.1b2 && \
 ./configure && \
 make && \
 make install

#Create tasks
RUN cp /CommonKmers/Docker/default /dckr/etc/tasks.d/default

#echo -e "\n \n NOTE: Be sure to download the training data at http://www.math.oregonstate.edu/~koslickd/CommonKmersData.tar.gz and uncompress it. The resulting directory should be passed to docker at the location CONT_DATABASES_DIR via: -v /path/to/uncompressed/Data:/dckr/mnt/camiref/CommonKmersData:ro"
