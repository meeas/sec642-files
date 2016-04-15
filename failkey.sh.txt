#!/bin/bash
# This script iterates through all the possible key values for the FailPics site,
# generating a hash from the combination of the "q" parameter (SQL query) and the
# key guess.  The output hash can then be compared to the hash from the URL
# parameter "md5", e.x.
# ./failkey.sh >hashes
# grep 189901be79c082ae7f6bc6dc0c0c0985 hashes

# This is the SQL select statement taken from the app, URL encoding removed
PARAM="SELECT image from failpics order by rand() limit 1"

# This is the hash value taken from the app "md5" parameter
HASH="189901be79c082ae7f6bc6dc0c0c0985"

# Iterate through all the possible values used to derive the key, focusing
# on 2011 since we know the site started in 2011.
for year in "2011"; do
    for month in `seq -w 01 12`; do        # The seq -w command produces 01 02 03 ... 12
        for day in `seq -w 01 31`; do      # The seq -w command produces 01 02 03 ... 31
            # Make sure to use "echo -n" to remove the newline before hashing!
            # The md5sum tool outputs the hash followed by "  -"; we strip that with sed
            keyguess=`echo -n "$year$month$day"| md5sum | sed 's/  -//'`
            hashguess=`echo -n $PARAM$keyguess | md5sum`
            echo "Key Guess: $keyguess ($year$month$day)	Hash Guess: $hashguess"
        done
    done
done
