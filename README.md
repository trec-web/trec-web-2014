trec-web-2013
=============

Please see the full guidelines at http://research.microsoft.com/trec-web-2013 for detailed
explanation of the TREC 2013 Web Track tasks and process.  This file explains what's in this github repository.

For training purposes, systems can work with previous years' adhoc topics using ClueWeb09. To aid this 
process we have provided in this github repository:
 1) Baseline runs over TREC Web Track 2012 and 2013 topics and collection (using ClueWeb09). 
 2) An updated version of standard TREC evaluation tools that compute risk-sensitive versions of 
 retrieval effectiveness measures, based on a provided baseline run.

1)  Baselines

For the risk-sensitive task, we provide a baseline run comprising the top 10000 results for a particular 
choice of easily reproducible baseline system. The risk-sensitive retrieval performance of submitted systems 
will be measured against this baseline. This year, the baseline will be provided using Indri with specific 
default settings as provided by the Lemur online service. 
 
Training baselines:

Our ClueWeb09 training baseline uses TREC 2012 Web track topics that uses the same Indri settings 
as will be used for the ClueWeb12 baseline. This baseline was computed using the Indri search engine, 
using its default query expansion based on pseudo-relevance feedback, with results filtered using 
the Waterloo spam filter. The github file with the TREC 2012 baseline run can be found off 
the trec-web-2013 root at:

/data/runs/baselines/2012/rm/results-cata-filtered.txt (ClueWeb09 full index, indri default relevance model, spam-filtered) 

For comparison we also provide other flavors of training baseline for CatB, and without spam 
filtering, in the same directory. 

We have also included simple query likelihood runs that do not use query expansion in 
/runs/baselines/2012/ql

Test baseline:

The official 2013 test baseline file has been released and can be found in the trec-web-2013 github repository at:

/data/runs/baselines/2013/rm/results-cata-filtered.txt

You should use this as the key baseline for optimizing your 2013 risk-sensitive submitted runs.  

This test baseline was computed using the same search system, retrieval method, and parameters as the corresponding 2012 training baseline: namely,
using the Indri search engine with default query expansion based on pseudo-relevance feedback, with results filtered using 
the Waterloo spam filter.

For further exploration, as with the training baselines, we have also provided various alternative flavors of test baseline, in case
you want to compare results, combine strategies, etc.  (The file naming mirrors those of the 2012 baselines.) However, only the rm/results-cata-filtered.txt baseline above will be used
as the final evaluation baseline.


2) Evaluation tools

This github repository also provides updated versions of standard TREC evaluation tools that compute 
risk-sensitive versions of retrieval effectiveness measures, based on a provided baseline run. These 
can be found in the trec-web-2013 github repository in the src/eval directory. 

There are two evaluation programs: ndeval, a C program which can be compiled to an executable,
and gdeval, which is written in Perl. The difference from last year's versions is a new baseline parameter, 
which if supplied, will compute the risk-sensitive evaluation measure based on the (also new) alpha 
parameter you provide.

To use ndeval, first build the executable using 'make' with the provided Makefile. 
ndeval requires a qrels.txt file, which contains the relevance judgements available from NIST. 
For this year, you will use the qrels file from TREC 2012, and the TREC 2012 baseline provided by us 
(described below) for training. To use measures that are backwards-compatible with last year, 
you just omit the baseline parameter. For example:
 
$ ./ndeval -c -traditional qrels.txt trec-format-run-to-evaluate.txt > normal-nd-evaluation.txt

For risk-sensitive measures, you add a -baseline file and a -riskAlpha setting. Remember the final risk 
weight is 1 + riskAlpha, i.e. riskAlpha = 0 corresponds to having no increased weight penalty for errors 
relative to the baseline and simply reports differences from the baseline. To evaluate with an increased 
weight on errors relative to the baseline, you could run for example:

$ ./ndeval -c -traditional -baseline trec-format-baseline-run.txt -riskAlpha 1 qrels.txt trec-format-test-run.txt > risk-sensitive-nd-evaluation.txt

Usage of gdeval is similar, with a new -baseline and -riskAlpha parameters. For backwards compatible evaluation like last year: 
 
$ ./gdeval.pl -c qrels.txt  trec-format-test-run.txt > normal-gd-evaluation.txt

To do a risk-sensitive evaluation with an increased weight on errors relative to the baseline, you could then do for example: 
 
$ ./gdeval.pl -c -riskAlpha 1 -baseline trec-format-baseline-run.txt qrels.txt  trec-format-test-run.txt >  risk-sensitive-gd-evaluation.txt

