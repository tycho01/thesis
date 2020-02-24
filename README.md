# UvA master thesis

type-driven development with hole-type-supervised NSPS / AlphaNPI? AlphaNSPS?

TODO (March 01):
- [x] research question
- [ ] literature review
- [ ] methodology
- [x] updated title
- [~] updated planning
- [/] any new information

### papers

sent by Emile:
- [ ] [DeepProbLog: Neural Probabilistic Logic Programming](https://arxiv.org/pdf/1805.10872.pdf) @ KU Leuven: differentiable [ProbLog](https://dtai.cs.kuleuven.be/problog/). Emile: less relevant cuz untyped? tests on add/sort/algebra on pics/list/text. induction, no ops.
- [x] [Learning Explanatory Rules from Noisy Data](https://arxiv.org/pdf/1711.04574.pdf) ([code](https://github.com/ai-systems/DILP-Core)) @ DeepMind: Differentiable Inductive Logic Programming (∂ILP) = ILP (data efficient) + NNs' ability to handle ambiguity. trained on various symbolic tasks. uses given template so just handles induction. check if this can do types? see pic on p.31. tests on [20 symbolic tasks](https://arxiv.org/pdf/1711.04574.pdf#page=27) (see appendix G: arithmetic, lists, family tree, graphs). task-dependent ops. not sure I can add value here if the template is set.
- [x] [DeepCoder](https://www.microsoft.com/en-us/research/publication/deepcoder-learning-write-programs/) (2017, repos [1](https://github.com/HiroakiMikami/deep-coder), [2](https://github.com/dkamm/deepcoder), [3](https://github.com/amitz25/PCCoder)): train a neural network to predict properties of the program that generated the outputs from the inputs to augment search techniques, e.g. enumerative search, SMT-based solver, depth-first search (DFS), 'sort and add' enumeration, sketch, λ^2. tested on functional programs. does not use a node tree though, so not amenable to my type-based approach.
- [ ] [Neural-Guided Deductive Search (NGDS)](https://www.microsoft.com/en-us/research/blog/neural-guided-deductive-search-best-worlds-approach-program-synthesis/) (2018):
    - combines symbolic logic + statistical models by 'branch & bound' from combinatorial optimization.
    - deductive search: decompose synthesis into sub-problems, Markovian so can do supervised learning.
    - NGDS: find which sub-problems are useful, predict program generalization by LSTM scoring DSL production rules on i/o.
    - evaluated on string transformation (PROSE) against RobustFill / DeepCoder.
    - deductive search seems kinda like pruning by type reasoning, though here it's learned. I guess the stronger the types tho, the less needs learning.
    - train separate models for different DSL levels, symbols, or even productions.
    - I *think* this seems iterative top-down, so seems applicable for me, tho no code.
    - this made me realize you may want to factor in existing operations on i/o to simplify the problem for the given sub-node.
    - sub-problems mentioned sounds relevant, and their previous paper FlashMeta (which this builds on) is mentioned in a [repo](https://github.com/reudismam/Refazer/blob/58f68f2c3f93e148a50f2eb09879a7bbe7fd6e3a/ProgramSynthesis/ProseFunctions/RelativePositioning/RightWitnessFunction.cs), though I'm not sure where this is imported from.
    - the used framework PROSE [offers holes](https://microsoft.github.io/prose/documentation/prose/usage/), implying they support the top-down incremental approach I'm hoping these papers are using.
    - from my understanding, from this paper (and the [FlashMeta paper](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/12/oopsla15-pbe.pdf)), my impression is that the approach used is more similar to bottom-up (though not narrowing down on one option at any level), prioritizing productions (then higher-level constructs) to direct a search. whereas I like the compositional nature of the bottom-up approach, for type purposes I appreciate that the top-down approach (as used by the NSPS paper) is type-bound by the desired input/output types.
    - PROSE's witness functions for constraint propagation look super interesting though! I kinda want this both for types and for input/output examples...
- [ ] [Recurrent Neural Network Grammars](https://arxiv.org/abs/1602.07776) (2016): probabilistic model of phrase-structure trees that can be trained generatively and used as a language model or a parser. seems aimed at language models, i.e. mimicking existing usage, which for program synthesis might be secondary to finding programs properly mapping input to output.
- [ ] [Leveraging Grammar and RL for Neural Program Synthesis](https://arxiv.org/abs/1805.04276) (2018): supervised + RL to generate semantically correct programs. train to generate syntactically correct programs that fulfill the specification. only applies for sequence approach, which doesn't match the trees I want...

mine (brought up):
- [x] [Neural Programmer-Interpreter (NPI)](https://arxiv.org/abs/1511.06279) @ DeepMind. tests on add/sort / canonicalizing 3d models. ops reset/bubble/rshift/lshift/compswap/ptr1l/ptr1r/ptr2l/ptr2r/swap/stop.
- [x] [Learning Compositional Neural Programs with Recursive Tree Search and Planning](https://arxiv.org/pdf/1905.12941.pdf) ([blog](https://www.instadeep.com/research-article/towards-compositionality-in-deep-reinforcement-learning/)): Neural Programmer-Interpreter (NPI) + AlphaZero = AlphaNPI. tests on sorting/hanoi tasks. ops as NPI.
- [x] [Neuro-Symbolic Program Synthesis (NSPS)](https://arxiv.org/pdf/1611.01855.pdf) (2016, no code): (1) cross correlation I/O network embeds in/out examples, (2) Recursive-Reverse-Recursive Neural Network (R3NN) incrementally synthesizes AST. R3NN has node representations, then calculates representations bottom-up then top-down to get representations for the next hole to fill. tests on flashfill. ops Concat/ConstStr/SubStr/Position/Direction/Regex.

mine (more):
- [x] [Typed meta-interpretive learning of logic programs](https://www.researchgate.net/profile/Andrew_Cropper/publication/331100541_Typed_meta-interpretive_learning_of_logic_programs/links/5c65c7bd299bf1d14cc75a39/Typed-meta-interpretive-learning-of-logic-programs.pdf) (2019, Prolog/Python [code](https://github.com/rolfmorel/jelia19-typedmil)): typed MIL (~= inductive logic programming (ILP)) reduces space by a cubic factor. tests on [list manipulation](https://github.com/rolfmorel/jelia19-typedmil/tree/master/experiments/03-evaluation). [ops](https://github.com/rolfmorel/jelia19-typedmil/blob/master/experiments/03-evaluation/nestedincr/gen_test_prolog.py#L43-L67). types: list<T>, int, char.
- [x] [Predicting Haskell Type Signatures From Names](https://people.cs.uchicago.edu/~rchugh/static/theses/bowen-wang-thesis.pdf) (2018): encode name/context, recursively select (non-)arrow
- [x] [Synthesizing Data Structure Transformations from Input-Output Examples](https://www.cs.utexas.edu/~isil/pldi15b.pdf) (2015, 145 cites, [code](https://github.com/jfeser/L2)): λ^2: OCaml+Python PBE from 7 combinators by inductive generalisation + (limited) deduction + enumerative search. typed, incremental, holes. old functional synthesis paper that got good improvement from types. tests on 40 data structure manipulation tasks (lists, trees, nested). ops: map, maptree, filter, foldl, foldr, foldtree, recl. 41 ops (lists, trees, nested).
- [x] [Type-and-example-directed program synthesis](https://dl.acm.org/citation.cfm?id=2738007) (2015, 100 cites, [code](https://github.com/psosera/thesis/blob/master/code/LambdaTermsCounts.hs)): Myth: Haskell/OCaml PBE by types + i/o. shows own synthesis/typecheck rules, library. tests on 40 (figure #9) + 5 (#10) functions ([details](https://github.com/psosera/thesis/blob/master/content/benchmark-suite.tex)). code repo unreleased. list/tree categories polymorphic, bool/nat aren't. eval during enum, standardize on beta-normal, typechecks partial functions, lists relevant math. top-down generates refinement trees by cycling through steps adding a type (type refinement) or potential values (guessing enumeratively) by examples. essentially constructs a simple idea that works for one example, check which other examples fail, and which parts of the program should be refined to fix those. pattern matches ADTs like lists (nil). enforces recursion. ppt claims ml-syn incomplete? lists several directions for future work. seems like a good paper, I should look for citing papers in case any has open-sourced a similar solution. should recheck if this does 'holes' though, as it seemed to generate complete programs?
- [x] [Programming Assistance for Type-Directed Programming](https://www.cs.grinnell.edu/~osera/publications/osera-tyde-2016.pdf) (2016, no code?): Scout: bridge auto-completion / program synthesis with interactive programming assistant for type-directed programming.
- [x] [Program Synthesis from Polymorphic Refinement Types](https://dl.acm.org/citation.cfm?id=2908093) (2016, [code](https://bitbucket.org/nadiapolikarpova/synquid)): code from refinement types. synthesize Haskell-like Synquid by SMT-solver from modular refinement type reconstruction to allow type-checking partial programs using liquid types.
- [x] [Top-Down Inductive Synthesis with Higher-Order Functions](https://www.research-collection.ethz.ch/bitstream/handle/20.500.11850/155821/eth-49631-01.pdf) (2016, [code](https://github.com/shasfin/ml4fp2016)): Tamandu: PBE in toy language from OCaml w/ large library (37 components), based on best-first enumeration combined with type-based pruning, evaluated on 23 functions (table 4.2).
- [x] [関係的仕様からの関数型プログラム合成](http://jssst.or.jp/files/user/taikai/2017/PPL/ppl3-1.pdf) (2017): relation-based (?) Haskell synthesis using Horn clauses.
- [x] [Suggesting Valid Hole Fits for Typed-Holes in Haskell](https://www.mpg.is/papers/gissurarson2018suggesting-msc.pdf) (2017, [code](https://github.com/Tritlo/ExampleHolePlugin)): integrates hole filler into GHC. mentions synthesizers insynth (JVM), [djinn](https://github.com/augustss/djinn/) (haskell, non-polymorphic). dependently typed languages: type inference undecidable. mentions hole-filling for agda and idris. great explanation of program synthesis on typed holes at 3.1.
- [ ] [Speeding up Type-Driven Program Synthesis with Polymorphic Succinct Types](https://icfp18.sigplan.org/getImage/orig/icfp18src-zheng-guo.pdf) (2018, [code](https://github.com/aaronguo1996/Synquid)): generalize succinct types for polymorphism. to find which types are used under polymorphism, abstract the types into coarse-grained succinct types, unordered param type sets describing function i/o.
- [x] [Automated Synthesis of Functional Programs with Auxiliary Functions](https://www-kb.is.s.u-tokyo.ac.jp/~koba/papers/aplas18-long.pdf) (2018): extension of Synquid to enable top-down iterative synthesis of programs with auxiliary functions using holes.
- [x] [Constraint-Based Type-Directed Program Synthesis](https://arxiv.org/pdf/1907.03105.pdf) (2019, no code?): synthesis from type. “infer while enumerate” approach to polymorphic type instantiation. uses WIP Haskell synthesis tool Scythe. no task!
- [x] [Temporal Stream Logic: Synthesis beyond the Bools](https://arxiv.org/pdf/1712.00246.pdf) (2017): CEGAR-based synthesis (from formal spec) separating control and data. benchmarks: synthesized music player Android app, controller for autonomous vehicle in the Open Race Car Simulator (TORCS).
- [ ] [Machine Learning for Combinatorial Optimization: a Methodological Tour d'Horizon](https://arxiv.org/abs/1811.06128) (2018).
    - is it useful to frame program synthesis as a [Combinatorial Optimization](https://paperswithcode.com/task/combinatorial-optimization) problem (not unlike the TSP)?
    - how do they differ? -- CO implies a loss/reward per configuration. for ML I should need a loss anyway tho, so this seems reasonable. the sub-field of linear programming requires linear relationships, which I doubt apply here.
    - what are the differences between their algorithms?
- [x] [HOUDINI: Lifelong Learning as Program Synthesis](https://arxiv.org/pdf/1804.00218.pdf) (2018, [code](https://github.com/capergroup/houdini)): NNs as typed differentiable functional programs to compose a library of neural functions to mix perception and procedural reasoning. library: NN, compose, map, fold, conv, on list/graph/tensor of bool/real. synth: generate (heuristically-guided top-down iterative refinement) + learn, evaluated on lifelong learning. suggests incorporating search strategies from NAS. not actually doing program synthesis tho... tests on several lifelong learning tasks (mix perceptual/algorithmic knowledge): recognize_digit, classify_digit, is_toy, regress_speed, regress_mnist, count_digit, count_toy, sum_digits, shortest_path_street, shortest_path_mnist. ops: hole, map, fold, convolution, compose.
- [x] [Idris, a General Purpose Dependently Typed Programming Language: Design and Implementation](https://eb.host.cs.st-andrews.ac.uk/drafts/impldtp.pdf) (2013, [code](https://github.com/idris-lang/Idris-dev)): covers language design including holes (section 4).
- [ ] [Abstract Syntax Networks for Code Generation and Semantic Parsing](https://arxiv.org/abs/1704.07535): uses LSTM to synthesize ASTs from natural language

notes:
- too little data
- without holes infer type of 'hole' as <T> -> T constraint (TS) / untyped argument (Haskell)?
- type filter wouldn't scale
- generate examples
- horn clause stuff: when a type doesn't match, know what layer mismatches to fix that? wait, I'd know from the start...
- compiler error counts as negative example
- how to get next suggestion in case of error? check how existing neural program synthesis does this...
