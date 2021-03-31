# Chess-Force-Data-Set

Data set created and used for the project on [https://github.com/fenilgmehta/Chess-Force](https://github.com/fenilgmehta/Chess-Force)

## Download instructions

- The PGN file have been taken from the following websites:
	* [https://www.kingbase-chess.net/](https://www.kingbase-chess.net/)
	* [http://utzingerk.com/test_kaufman](http://utzingerk.com/test_kaufman)
    * Kaufman's 25 boards postions: [https://www.chessprogramming.org/Kaufman_Test](https://www.chessprogramming.org/Kaufman_Test)
- One may either download files from `03_csv_score_data` or `03_csv_score_data_compressed`. The only difference is that `03_csv_score_data` contains raw CSV files, whereas `03_csv_score_data_compressed` contain the same files in compressed ZIP format.
- | File/Folder                    | Size    |
  | ------------------------------ | ------- |
  | `03_csv_score_data`            | 1.68 GB |
  | `03_csv_score_data_compressed` | 0.6  GB |
- | PGN source                                | Game count |
  | ----------------------------------------- | ---------- |
  | KingBase2019-pgn/KingBase2019-A00-A39.pgn | 58968      |
  | KingBase2019-pgn/KingBase2019-A40-A79.pgn | 35706      |
  | KingBase2019-pgn/KingBase2019-A80-A99.pgn | 7020       |
  | KingBase2019-pgn/KingBase2019-B00-B19.pgn | 38419      |
  | KingBase2019-pgn/KingBase2019-B20-B49.pgn | 44497      |
  | KingBase2019-pgn/KingBase2019-B50-B99.pgn | 29985      |
  | KingBase2019-pgn/KingBase2019-C00-C19.pgn | 23555      |
  | KingBase2019-pgn/KingBase2019-C20-C59.pgn | 25927      |
  | KingBase2019-pgn/KingBase2019-C60-C99.pgn | 20437      |
  | KingBase2019-pgn/KingBase2019-D00-D29.pgn | 31801      |
  | KingBase2019-pgn/KingBase2019-D30-D69.pgn | 25397      |
  | KingBase2019-pgn/KingBase2019-D70-D99.pgn | 19695      |
  | KingBase2019-pgn/KingBase2019-E00-E19.pgn | 24082      |
  | KingBase2019-pgn/KingBase2019-E20-E59.pgn | 12034      |
  | KingBase2019-pgn/KingBase2019-E60-E99.pgn | 22579      |
  | Kaufman games                             | 25         |
  
  ∴ Total chess games = 420127

## Dataset info

- Stockfish 10 was used for the score generation

- Stockfish 10 configuration:
	* Max CPU cores/threads = 1
	* Max Hash table size = 16 MB
	* Max search depth = 20
	* Max thinking time = 1 second(for KingBase), 2 seconds(for Kaufman)
	
- The output of Stockfish 10 is normalized using the method described in the paper [Predicting Chess Moves with Multilayer Perceptron and Limited Lookahead](http://ijera.com/papers/vol10no4/Series-1/B1004010508.pdf) under the section `III Methodology - A. Dataset, centipawn score generation and normalization, and board representation`, and then it is divided by `𝑚𝑎𝑥_𝑠𝑐𝑜𝑟𝑒` so that the score is between -1 and 1.

  - Formula used if it is a checkmate `𝑐𝑝 = 𝑚𝑎𝑥_𝑠𝑐𝑜𝑟𝑒 − 𝑚𝑎𝑡𝑒_𝑠𝑡𝑒𝑝𝑠 ∗ 𝑚𝑖𝑛_𝑑𝑖𝑓 ∗ 𝑠𝑖𝑔𝑛`
    - 𝑐𝑝 = centi-pawn score
    - 𝑚𝑎𝑥_𝑠𝑐𝑜𝑟𝑒 = 10,000
    - 𝑚𝑎𝑡𝑒_𝑠𝑡𝑒𝑝𝑠 = it’s the absolute value of the number of steps in which one of the sides can be checkmated
    - 𝑚𝑖𝑛_𝑑𝑖𝑓 = 50
    - 𝑠𝑖𝑔𝑛 = +1 if black is getting checkmated, -1, if white is getting checkmated
  - If it is not a checkmate, then the score is directly used and divided by `𝑚𝑎𝑥_𝑠𝑐𝑜𝑟𝑒`.

- The CSV files are in the following format:

  - First line is `fen_board,cp_score`

  - All line after that are: `FEN notation of the chess boards,centipawn-score`

  - The first few lines of [03_csv_score_data/z__r_A00-A39__aa.csv](./03_csv_score_data/z__r_A00-A39__aa.csv) are:
  
    | fen_board                                                         | cp_score |
    | ----------------------------------------------------------------- | -------- |
    | 2r5/p3prb1/6kp/1PNp2p1/P2P4/6PP/1B2RPK1/8 b - - 0 33              | -0.0187  |
    | 5r1k/pQ4pp/3p2b1/8/2P3q1/1PN3P1/P5BP/4R2K w - - 1 31              | 0.0824   |
    | rnbq1rk1/ppp1bppp/4pn2/3p4/8/3P1NP1/PPPNPPBP/R1BQ1RK1 b - - 2 6   | -0.0059  |
    | rnbq1rk1/ppp1npbp/6p1/4p3/1PPp4/3P1NP1/P2NPPBP/R1BQ1RK1 b - - 2 8 | 0.0029   |
    | 8/8/1B4k1/6p1/5p1p/8/8/6K1 b - - 2 74                             | -0.0825  |

