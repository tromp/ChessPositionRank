# Chess Position Ranking

| What do | these chess | positions | have in | common ? |
| ------- | ----------- | --------- | ------- | -------- |
| ![png](img/503289063779196128361150689047367567872372294.png)  | ![png](img/580140679043901186888998342254951860638526439.png)  | ![png](img/593137308386226658094145754569325533779042325.png)  | ![png](img/991549227431426157096206009465592604110845703.png)  | ![png](img/1047525017269244678197701496747267619789198017.png) |
| ![png](img/1083036227884941701861354854490530917455771815.png) | ![png](img/1106489605984601488532349085198410170621706348.png) | ![png](img/1147708700602021734545135507635946335124185282.png) | ![png](img/1154734958516807063801074729753750650904416235.png) | ![png](img/1158796981116594223045771184092939646123946906.png) |
| ![png](img/1209701759591132302906184345657908383841824235.png) | ![png](img/1725484852290730636030896867022217870228868704.png) | ![png](img/1784693133099122184545632700841496633418777464.png) | ![png](img/1856606560821136739817670996927038578648392221.png) | ![png](img/2257065082488789417141112868814925331867976479.png) |
| ![png](img/2315344891160709239126870145978365388427159117.png) | ![png](img/2389704906374985477664262349386869232706664089.png) | ![png](img/2562648667168074067829468650669534536867027058.png) | ![png](img/2718238962736887748596159406933027452923729947.png) | ![png](img/3055629651844565686947192031215016583849102728.png) |
| ![png](img/3398606048446236323653246601122615928968616440.png) | ![png](img/3425365939195111838766560694320233171380129055.png) | ![png](img/4399584198957353812896813114791774751467478074.png) | ![png](img/4762194553027923162499134554477258030628304096.png) | ![png](img/5293733241256913999408551583484086355358325239.png) |
| ![png](img/5429450993601644945682336365610106653989111795.png) | ![png](img/5639611074235380402252180134204464211964938593.png) | ![png](img/5647448437270439444982600541786209886576225682.png) | ![png](img/5691504722035780516541128373989374510644277936.png) | ![png](img/5715889986081721048428563847037002364334037126.png) |
| ![png](img/5781922138500968236153615806061782133526228898.png) | ![png](img/5927604647784692973221548766230048836596452045.png) | ![png](img/5961477550017369146609798743503413122905087621.png) | ![png](img/5972169225015582249741929782314132324997244418.png) | ![png](img/6095794971871435537562570737927159417850903478.png) |
| ![png](img/6248683039021827353357047172082732510290958040.png) | ![png](img/6394829818128653476475598910823681678434345611.png) | ![png](img/6467386408575782931904566422867761755281600406.png) | ![png](img/6527004687905500436894156281614056395711734439.png) | ![png](img/6559049323102662377467679186753286679840563175.png) |
| ![png](img/6767070742059246616395143362223913130453378060.png) | ![png](img/7418706063376707252977262020406376172074149082.png) | ![png](img/7560181429608366042251170533318351562860121740.png) | ![png](img/7657222361144733285184648220461781235138296736.png) | ![png](img/7953115162615822573584189721136822725600875337.png) |
| ![png](img/7973341163633784542498891264685208315995622129.png) | ![png](img/8260130656505158220074292709508749561113581601.png) | ![png](img/8407831187507152187492557297490243416405059949.png) | ![png](img/8545434809537180765086507091416427802529352632.png) | ![png](img/8624116876876796351697627721325589737238505925.png) |

Not shown in the diagrams is that the first 22 have White to move, the remaining 28 have Black to move.

# These 50 positions have two things in common

1. They're all legal. That is, reachable from the starting position in a valid game of chess.

2. They're randomly generated.  
In fact, these 50 are all the legal positions among a sample generated by picking 1000 random integers in a specific range, and converting each integer into a corresponding position.
Hovering over each position will show the integer used to generate it.

# Rankings

The conversion from integers to positions is made possible by a so-called *ranking*,
which is a triple consisting of a size, 
an unrank function that maps integers in the range 0..size-1 to objects,
and a rank function that maps objects to integers.
A ranking is invertible when the rank and unrank functions form a bijection, giving each object a unique rank.

The objects in our chess ranking are actually not mere positions, but positions annotated with some additional data generated during the unranking process. For simplicity, we call them uniquely rankable positions, or urpositions for short.
Each chess position can occur as multiple *equivalent* urpositions, giving it an associated *multiplicity*.

This Haskell based software suite implements an invertible ranking of N = 8726713169886222032347729969256422370854716254 (~ 8.7E45) urpositions.
The main executable, cpr, uses [Forsyth–Edwards Notation](https://en.wikipedia.org/wiki/Forsyth%E2%80%93Edwards_Notation) for input and output of urpositions, with the following modifications:
 * the en-passant field is only used with an opponent pawn present to capture
 * the last two fields record multiplicity and the rank of the urposition in a ranking of equivalent ones.

# Estimating the number of legal positions

The 50 legal positions have an average multiplicity of 54/50~1.08, slightly higher than the 1.052 average of all 1000.
With a 95% [confidence level](https://en.wikipedia.org/wiki/Binomial_proportion_confidence_interval#Normal_approximation_interval), this yields an estimated number of legal positions of (5% +- 1.96 sqrt(5% * 95% / 1000)) * N / 1.08, or (4+-1.1)x10^44.

# Improving accuracy

The estimate above has less than one digit of accuracy.
More accuracy can be obtained by analysing the larger samples
testRnd10kResearch, testRnd100kResearch, or testRnd1mResearch, with the first
giving 1 digit of accuracy and the last
one giving 2 digits of accuracy at the 95% confidence level.
Analyzing the larger 100k and 1m samples will probably require
additional software to aid and/or distribute the analysis effort.

# Preliminary 10k results

Under milestone "10k", I have filed a separate issue for each of the 919 potentially legal positions in the 10k sample (which extends the 1k sample with 9k new positions).

Mario Richter kindly contributed the output of his legality checking program "rawbats", which found 130 of the 919 positions to be illegal. Of the remainder, I manually judged 538 to be legal and 381 to be illegal. These 538 positions have an average multiplicity of 561/538~1.04275, slightly larger than the 1.0355 average of all 10000.
If my judgement is correct (a big if) this yields an **estimated number of legal positions of** (5.38% +- 1.96 sqrt(5.38% * 94.62% / 10000)) * N / 1.04275, or **(4.5 +- 0.37) x 10^44**.

# Chess players wanted

Chess players are invited to contribute to this project by joining [github](https://github.com/), picking an open [issue](https://github.com/tromp/ChessPositionRanking/issues) and using [lichess](https://lichess.org/analysis) to construct a [Proof Game](https://github.com/tromp/ChessPositionRanking/issues/464) leading to the position in question. This will establish with absolute certainty that the position is legal. Only 24 of the 538 have proof games constructed for them so far. Contributors of at least 10 proof games will receive acknowledgement in an eventual publication, as will anyone finding an misclassified position.

Unfortunately, judgements of illegality cannot be established with absolute certainty. Chess players familiar with retrograde analysis are invited to randomly check purported proofs of illegality.
They range in complexity from the [very simple](https://github.com/tromp/ChessPositionRanking/issues/98) through the [medium complex](https://github.com/tromp/ChessPositionRanking/issues/22) to the [quite complex](https://github.com/tromp/ChessPositionRanking/issues/136),
and often rely on so-called statistics of white and black armies. These stats include men captured (x), number of pawns (p), minimum number of promotions required (pr), number of pawns captured (px), maximum number of unopposed pawns (maxup), and minimum number of opposing pawn files (minopp).
The key inequality is that the number of white (resp. black) promotions is limited by the number of black (resp. white) pawns captured plus the total number of captures. A pair of adjacent files each with a white pawn opposing a black pawn support 3 promotions by having one pawn capture the opponent in the other file. A captured piece supports 2 promotions on a single file with a white pawn opposing a black pawn by letting either pawn capture the piece to an adjacent file where it is no longer opposed.

# Reproducibility

We produced our integer samples using Haskell's built-in random number
generator with a seed of 0, and sorted the result for use in batched ranking
(Makefile targets testRnd1kRanks, testRnd10kRanks, and testRnd100kRank).

We unranked these to obtain random urpositions (Makefile targets testRnd1kFENs,
testRnd10kFENs, and testRnd100kFENs). NOTE that these can take over half an hour to complete.

We checked invertibility by ranking the results (Makefile targets testRnd1kRanking, testRnd10kRanking, and testRnd100kRanking), and comparing with the original integers used for unranking.

We ran some legality tests that find ~95% of the positions to be definitely
illegal (Makefile targets testRnd1kResearch, testRnd10kResearch,
testRnd100kResearch).

The file sortedRnd1kResearchManual extends sortedRnd1kResearch with a manual determination of legality of the 93 remaining positions.

# Related Work

Estimates of the number of legal chess positions date back to Claude Shannon's [seminal 1950 paper](https://vision.unipv.it/IA1/ProgrammingaComputerforPlayingChess.pdf).

The reason for satisfying ourselves with a mere estimate is that we have no
hope of ever determining the number exactly, like we did in the [game of Go](https://tromp.github.io/go/legal.html).
Whereas in Go, deciding whether a given position is legal is quite simple,
determining the legality of a given chess position is exceedingly hard. It is
the essence of so-called [retrograde analysis problems](https://en.wikipedia.org/wiki/Retrograde_analysis) that concern the
possible past moves rather than the possible future moves that normal chess
problems concern themselves with.

The reader is invited to study some of the best retrograde problems ever
composed, together with their intricate solution, at this [``Masterworks''
collection](https://www.janko.at/Retros/Masterworks/Part1.htm). Evidence of the algorithmic hardness of retrograde problems may
be found in this recent [computational complexity paper](https://arxiv.org/abs/2010.09271) that proves
retrograde decision problems on arbitrarily large chess boards to be
PSPACE-hard.

In his 1994 [PhD thesis](http://fragrieu.free.fr/SearchingForSolutions.pdf), Victor Allis claims a calculated upper bound of
5E52 and assumed "the true state-space complexity to be close to 1E50".
We suspect a programming bug led to these numbers being many orders of magnitude
too large.

In 1996, Shirish Chinchalkar obtained an upper bound of
[1.77894E46](https://www.chessprogramming.org/ICGA_Journal#19_3).  A copy of
the program that Shirish Chinchalkar developed to compute his upperbound may be
found in Will Entriken's [github
repo](https://github.com/fulldecent/chess-upper-bound-armies), which also
contains his software for ranking chess diagrams.

# Bug Bounties

Since validity of these results hinges on the ranking including all legal positions, a bounty of $256 is hereby offered to the first person to file an issue with a legal but unrankable position. A bounty of $128 is offered for a rankable position for which unranking reports the wrong multiplicity.

# Interesting observations

The upper bound of N = 8726713169886222032347729969256422370854716254
is computed in under 10s by [CountChess.hs](src/CountChess.hs) (Makefile target count).

The original ranking implementation based on [Data/Ranking.hs](src/Data/Ranking.hs) took on the order of 10s per position on top of the startup time of around 30 mins.
Adding the batched rankings of [Data/Ranking/Batched.hs](src/Data/Ranking/Batched.hs) resulted in huge speedups where many millions of positions could be generated within an hour.

The entire urposition ranking is composed out of 14 separate functions called

    sideToMoveRanking
    caseRanking
    wArmyStatRanking bArmyStatRanking
    guardRanking
    enPassantRanking epOppRanking sandwichRanking
    opposeRanking pawnRanking castleRanking wArmyRanking bArmyRanking pieceRanking

The input to each function is some data representing earlier choices, while the output is a ranking of further choices.
Functions on the same row compose like a cartesian product, i.e. making choices independent of each other. The efficiency of the entire ranking is mostly attributable to the large final row.

The maximum multiplicity of 2 * 70 = 140 is achieved on positions where 4
pieces but no pawns were captured (courtesy of opposeRanking),
and with an en-passant pawn
landing between two opponent pawns (courtesy of enPassantRanking).
For example, unranking
4363356584943110212154534698526026458664867108 yields FEN output
"r3kb1r/ppp1pp1p/3p4/5PpP/8/8/PPPPP1P1/RNBQKBNR w KQkq g6 99 140".

Treating placement of kings just like that of other pieces hugely simplifies the ranking,
but allows positions with adjacent kings. Avoiding adjacent kings only improves the upper bound by 10%, which is not worth the complications and resulting slowdowns,

Most urpositions (72.5%) have 4 captures, which supports up to 12 promotions.
The next most common capture counts are 5 (20%), 3 (5.7%), 6 (1.9%) and 7 (0.1%).

The number of promotions is much more evenly distributed, with 4 through 12 promotions accounting for
0.3%, 1.1%, 3.4%, 8.3%, 15.2%, 21.5%, 22.7%, 17.1%, 8.3%, and 2% of random urpositions.

The 1 million sample classifies as

    $ src/legal < sortedRnd1mFENs | grep "^ " | sort | uniq -c | sort -rn
    492045  Illegal Both Kings in Check
    173401  Illegal Side not to move in Check
    102541  Illegal Adjacent Kings
    79019  Illegal Bishops Too Monochromatic
    53063  Single Check
    35584  No Checks
    29962  Illegal Triple Check
    27541  Illegal Double Check
    6130  Discovered Double Check
     714  Illegal Double Knight Check

leaving 53063 + 35584 + 5839 = 94486 positions for manual analysis.

Some people might enjoy minimizing the number of moves in a Proof Game, in what could be a friendly "Chess golf" competition, similar to [Code golf](https://en.wikipedia.org/wiki/Code_golf).

# References

[0] https://en.wikipedia.org/wiki/Forsyth%E2%80%93Edwards_Notation

[1] https://en.wikipedia.org/wiki/Binomial_proportion_confidence_interval#Normal_approximation_interval

[2] https://vision.unipv.it/IA1/ProgrammingaComputerforPlayingChess.pdf

[3] https://tromp.github.io/go/legal.html

[4] https://en.wikipedia.org/wiki/Retrograde_analysis

[5] https://www.janko.at/Retros/Masterworks/Part1.htm

[6] https://arxiv.org/abs/2010.09271

[7] http://fragrieu.free.fr/SearchingForSolutions.pdf

[8] https://www.chessprogramming.org/ICGA_Journal#19_3

[9] https://github.com/fulldecent/chess-upper-bound-armies
