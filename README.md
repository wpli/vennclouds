Dynamic Wordclouds and Vennclouds
Developed at the Human Language Technology Center of Excellence, Johns Hopkins University
Author: Glen Coppersmith (coppersmith@jhu.edu)
Author: Erin Kelly (elkelly8@gmail.com)
Author: Aleksander Yelskiy (ayelskiy@gmail.com)

If you use this, please cite the following paper:
Glen Coppersmith and Erin Kelly (2014). Dynamic Wordclouds and Vennclouds for Exploratory Data Analysis.
Association for Computational Linguistics Workshop on Interactive Language Learning and Visualization.


---
Quick Start
---

```
# Generate IDF vector
python create_idf_vector.py --files shakespeare/* --output mytitle.json

# Generate interactive web page
python dynamic_wordclouds.py shakespeare/* --idf mytitle.json --output shakespeare.html
```

[[This README is currently out of data, for usage instructions run:
python dynamic_wordclouds.py --help ]]


This is _RESEARCH_ code, there are likely still bugs and gotchas hiding in the nooks and crannys. If you find one, please do let us know.

NB: this generates (potentially large) encapsulated HTML files, that can be used when disconnected from the internet. Please be certain that you place the generated HTML files in a directory that contains the `offline_source' directory, so the HTML files can find the jQuery and other libraries used.

---
dynamic_wordclouds.py
---
This houses all of the functions (importable as python function calls) required to make dynamic wordclouds and Vennclouds.
For usage instructions:

```bash
python dynamic_wordclouds.py -h
```

usage:

```bash
       dynamic_wordclouds.py [-h] [--output OUTPUT] [--idf IDF]
                             [--examples EXAMPLES] [--window WINDOW]
                             [--minimum-frequency MINIMUM_FREQUENCY]
                             N [N ...]
```

Create a Venncloud html file.

positional arguments:
  * `N`                 Location of the documents for the datasets to be
                        loaded -- plain text, 1 document per line.

optional arguments:
  *  `-h, --help`           show this help message and exit
  *  `--output OUTPUT`      Where the output html file should be written.
  *  `--idf IDF`            Location of an idf vector to be used, as a JSON file
                            of a python dictionary -- see `create_idf_vector.py`
                            to make one. If this argument is omitted, we will
                            generate the idf vector from the provided documents.
  * `--examples EXAMPLES`   Number of examples of each word to store [defaults to
                            5].
  * `--window WINDOW`       Window size on each side for each example, in number
                            of tokens [defaults to 5].
  * `--minimum-frequency MINIMUM_FREQUENCY`
                        Minimum occurences of a word included in the venncloud
                        data [defaults to 3].

---
create_idf_vector.py
---
This will create and store an IDF vector from an arbitrary number of text files (one document per line [specify as `docs`] or one document per file [specify as `files`]). This is best run on a large number of documents, so as to get a good estimate of the true IDF value of each token. The resulting JSON file can then be passed into dynamic_wordclouds.py to create the wordclouds.
For usage instructions:

```bash
python create_idf_vector.py -h
```

usage:

```bash
       create_idf_vector.py [-h] [--output OUTPUT] [--docs [DOCS [DOCS ...]]]
                            [--files [FILES [FILES ...]]]
```

Create a JSON idf vector for use with the dynamic wordclouds.

optional arguments:
  * `-h, --help`        show this help message and exit
  * `--output OUTPUT`   Where the output JSON file should be written.
  * `--docs [DOCS [DOCS ...]]`
                        List of text files used to generate the idf vector --
                        one document per line, multiple text files allowed
  * `--files [FILES [FILES ...]]`
                        List of text files used to generate the idf vector --
                        one document per text file, multiple text files
                        required


---
Example Usage:
---

1) Simple usage:

```bash
python dynamic_wordclouds.py --output output_location.html doc1.txt doc2.txt doc3.txt
```

where `doc1.txt`, `doc2.txt.`, `doc3.txt` are text documents, one document per line. This will calculate IDF and the wordcloud from the same set of documents (this is only advised if there are a large number of documents in this text file), and store the output at `output_location.html`.

2) Create an idf vector for use later:

```bash
python create_idf_vector.py  --output this_idf_vector.json blue.txt red.txt some_text_file.txt
```

where `blue.txt', `red.txt' and `some_text_file.txt' are plain text documents with one document per line. 

3) Use the idf vector previously created to make a wordcloud and store it at venn_cloud.html

```bash
python dynamic_wordclouds.py --idf this_idf_vector.json --output example_venncloud.html doc1.txt doc2.txt doc3.txt
```

where `doc1.txt`, `doc2.txt`, and `doc3.txt`  are plain text documents with one document per line, `this_idf_vector.json` was generated by `create_idf_vector.py` as above. 

4) Additional parameters:
Some of the internal parameters are exposed to the command-line interface:
  * `--output` specifies where the output.html file should be written
  * `--idf` specifies where a JSON IDF vector should be loaded from (created by `create_idf.py`).
  * `--examples` specifies the maximum number of examples to keep for each word token observed.
  * `--window` specifies the number of token to either side of a given token that is stored for each token example.
  * `--minimum-frequency` specifies the minimum number of times a token must be observed in order to be included in this analysis.
If the html file generated is too large, turning down `examples`, `window` and turning up `minimum-frequency` may help.
