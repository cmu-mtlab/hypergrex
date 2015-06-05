HyperGrex
=========

A hypergraph-based syntactic translation grammar extractor for use with [cdec](http://www.cdec-decoder.org/) and similar translation systems.

## Supported functionality

 * extract various kinds (tree-to-string, tree-to-tree, string-to-tree) of tree transduction rules from aligned parallel corpora with parses on one or both sides
 * tranduction rules can be *minimal* or *composed*, with limits on the size and complexity of the rules
 * extract rules from parse forests or (*k*-best) lists of parses
 * score extracted rules with a variety of standard features

## Example tree-to-string extraction

    python hg_rule_extractor.py test_data/test.fr test_data/test.en test_data/test.al --t2s -m -s 1000 > rules.t2s

The options are:
 * `test_data/test.fr` is the source side of the bitext, parsed, one tree per line
 * `test_data/test.en` is the target side of the bitext, one sentence per line (not parsed)
 * `--t2s` indicates that xRs rules should be extracted
 * `-m` indicates that *minimal* (non-composed) rules should be extracted
 * `-s 1000` indicates that rules my have up to 1000 symbols in them (effectively, this disables any size-based filtering)

The above command writes the following rules to the file `rules.t2s`:

    (PP [P] (NP (DT l') [NN])) ||| [1] [2] ||| 0-0 2-1 ||| count=1.0 sent_count=1
    (VP (VB a) [VBN] [PP]) ||| [1] [2] ||| 1-0 2-1 ||| count=1.0 sent_count=1
    (P à) ||| to ||| 0-0 ||| count=1.0 sent_count=1
    (S [NP] [VP] [PUNC]) ||| [1] [2] [3] ||| 0-0 1-1 2-2 ||| count=1.0 sent_count=1
    (PUNC .) ||| . ||| 0-0 ||| count=1.0 sent_count=1
    (VBN marché) ||| walked ||| 0-0 ||| count=1.0 sent_count=1
    (JJ petit) ||| young ||| 0-0 ||| count=1.0 sent_count=1
    (DT le) ||| the ||| 0-0 ||| count=1.0 sent_count=1
    (NP [DT] [JJ] [NN]) ||| [1] [2] [3] ||| 0-0 1-1 2-2 ||| count=1.0 sent_count=1
    (NN école) ||| school ||| 0-0 ||| count=1.0 sent_count=1
    (NN garçon) ||| boy ||| 0-0 ||| count=1.0 sent_count=1

## Adding features to rules

    ./t2s_score/score.sh rules.t2s test_data/sgt-params.txt test_data/tgs-params.txt

The second and third files are lexical translation probabilities.

## For further information

 * For information on tree-to-string (xRs) translation rules, see
   * [the `cdec` documentation](http://www.cdec-decoder.org/concepts/xrs.html)
   * [this paper by GHKM](http://www.aclweb.org/anthology/N/N04/N04-1035.pdf), and
   * [this paper by Liang Huang](http://www.cis.upenn.edu/~lhuang3/amta06-sdtedl.pdf)
   * [this follow up paper by Galley et al.](http://www.cs.columbia.edu/nlp/papers/2006/galley_al_06.pdf).
   * [this dissertation by Jon May](http://www.isi.edu/~jonmay/pubs/thesis_ss.pdf)
 * For more information on the supported tree-to-tree formalism, see
   * this paper

This software is a rewrite of the [Grex grammar extractor](https://github.com/ghanneman/grex)

