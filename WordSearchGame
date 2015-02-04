
import java.io.*;

public class WordSearchGame {

    // array that holds the correct user guesses
    static String[] correctUserGuesses;

    // number of correct guess
    static int number;

    // global variable that creates an "In" object to read the NxN matrix
    static In dataBoard;

    // global variable that 
    static int dimension;

    // global variable that creates an "In" object for the two dimensional array board
    static String[][] myBoard;

    // TST that holds words found in the word search 
    static TST foundWords = new TST();

    // TST that holds the dictionary...used to check if the word found in the board is valid
    static TST myTST;

    // Bag used to hold the dictionay when it is read in
    static Bag<String> bag = new Bag<String>();

    static Bag<String> userBag = new Bag<String>();

    // stopwatch used to time appropriate operations
    static Stopwatch sw;

    // display for the columns
    static int display = 0;

    public static void main(String args[]) {

        // if statement that handles all the user input from the command line
        // including "-dict" "-board" "-cols"
        // also sets the "-dict" to dict10.txt is the user does not specify the dictionary
        if (args.length > 0) {
            for (int i = 0; i < args.length; i++) {
                // The user enters "-help" the follwing print statements will be executed 
                if (args[i].equals("-help")) {
                    StdOut.print("\nWelcome to WORD SEARCH! Run with \"-help\" for cmd-line options");
                    StdOut.print("\nOptions: ");
                    StdOut.print("\n'-board FILENAME': Specifie game board file.");
                    StdOut.print("\n'-dict FILENAME' : Specifies dictionary file");
                    StdOut.print("\n'-cols NUMCOLS' : Specifies the number of colimns for printing words");
                    System.exit(0);
                }
                if (args[i].equals("-board")) {
                    String file = args[++i];
                    // "In" class and creates the object databoard
                    dataBoard = new In(new File(file));
                    // gets the dimensions for the NxN matrix
                    dimension = Integer.parseInt(dataBoard.readLine());
                    // Sets the dimensions of "myBoard"
                    myBoard = new String[dimension][dimension];
                    // fills "myBoard" with Strings from the databoard object
                    for (int row = 0; row < dimension; row++) {
                        for (int col = 0; col < dimension; col++) {
                            String n = dataBoard.readString();
                            myBoard[row][col] = n;
                        }
                    }
                } else if (args[i].equals("-dict")) {
                    myTST = new TST();

                    // "fin" is the In object that reads in the dictionary
                    In fin = new In(args[++i].toString());
                    int count = 0;

                    // Stopwatch used to time filling the bag from dick
                    sw = new Stopwatch();
                    // while loop reads the dictionary file into a bag object
                    while (!fin.isEmpty()) {
                        String n = fin.readLine();
                        bag.add(n);
                    }
                    StdOut.println();
                    StdOut.println("Reading dictionary" + " (" + bag.size() + ") " + "from disk to a bag: " + sw.elapsedTime());

                    // Stopwatch used to time filling the myTST from the bag
                    sw = new Stopwatch();
                    // for each loop places from the bag object to myTST
                    for (String s : bag) {
                        myTST.put(s, count);
                    }

                    StdOut.println("Putting dictionary into ternary-search-trie: " + sw.elapsedTime());

                } else if (args[i].equals("-cols")) {
                    // reads the user input for cols
                    display = Integer.parseInt(args[++i]);
                }

                // if the user does not specify the a dictionary
                // this if statement defaults the dict to "dict10.txt"
                if (myTST == null) {
                    myTST = new TST();
                    StdOut.println("dict10.txt");

                    sw = new Stopwatch();
                    In fin = new In("dict10.txt");
                    while (!fin.isEmpty()) {
                        String m = fin.readLine();
                        bag.add(m);
                    }
                    StdOut.println("Putting dictionary into ternary-search-trie: " + sw.elapsedTime());
                }

                // set the default columns to 6
                if (display == 0) {
                    display = 6;
                }

            }
        }

        boolean play = true;
        do {
            StdOut.print("\nWelcome to WORD SEARCH! Run with '-help' for cmd-line options");
            // print the board 
            StdOut.println("\nGame Board: ");
            for (int row = 0; row <= dimension - 1; row++) {
                for (int col = 0; col <= dimension - 1; col++) {
                    StdOut.print(myBoard[row][col]);
                    // the if statement changes all "*" symbols to "." on the board
                    if (myBoard[row][col].equals("*")) {
                        myBoard[row][col] = (".");
                    }
                }
                StdOut.println();
            }

            // navigate the board from point to point
            // all the points on the board are sent to "checkTheBoard" method
            for (int i = 0; i <= dimension - 1; i++) {
                for (int j = 0; j <= dimension - 1; j++) {
                    checkTheBoard(myBoard, i, j);
                }
            }

            StdOut.println("\nYou can now type words. Type 'END' and hit ENTER to quit\n"
                    + "playing the list all the words on the game board\n");
            userQuestions();

            // List all the words that are in both the dict and the matrix
            int counter = 0;
            StdOut.println("\nList of all words on the game board: ");
            Iterable<String> keys = foundWords.keys();
            for (String key : keys) {
                if (counter % display == 0) {
                    StdOut.println();
                }
                StdOut.printf("%s\t", key);
                counter++;
            }
            StdOut.println();

            int otherCounter = 0;
            StdOut.println("\nList of the words you found: ");
            for (String k : userBag) {
                if (otherCounter % display == 0) {
                    StdOut.println();
                }
                StdOut.printf("%s\t", k);
                otherCounter++;
            }
            StdOut.println();

            // prompt user if they would like to play again
            StdOut.println("\nPlay again? [Y/N]");

            // Read the users input
            String playAgain = "";
            playAgain = StdIn.readString();
            playAgain.toLowerCase();

            if (playAgain.equals("n") || playAgain.equals("N")) {
                play = false;
            }

        } while (play);

    }

    public static void userQuestions() {
        
        // Reads the users input of words they guess exist in the board and dictionary
        String userInput = "";
        userInput = StdIn.readString();
        while (!userInput.equals("END")) {
            
            // Creates an Iterable TST and adds correct user gues to a bag
            Iterable<String> guess = foundWords.keys();
            for (String s : guess) {
                if (userInput.equals(s)) {
                    userBag.add(userInput);
                }
            }
            
            // tell the user if there guess is correct or incorrect
            if (foundWords.contains(userInput)) {
                StdOut.println("Yes, " + userInput + " is in the dictionary and on the game board.");
                userInput = StdIn.readString();
            } else {
                StdOut.println("No, " + userInput + " is not in the dictionary and/or game board, guess again slick.");
                break;
            }
        }
    }

    // This method searches the board
    // it contains while loops for each possible direction
    // N, S, E, W, NW, NE, SW, SE
    // After a String had been concatinated and it is longer that
    // it is sent to the method "ifWordExistsInTST" to see if it occurs
    // in the TST
    public static void checkTheBoard(String[][] board, int y, int x) {
        int valueCounter = 0;
        int tempX = x;
        int tempY = y;

        // Strings that get concatinated for each direction
        String wordNorth = "";
        String wordSouth = "";
        String wordWest = "";
        String wordEast = "";
        String wordNE = "";
        String wordSW = "";
        String wordNW = "";
        String wordSE = "";

        // ints that are assigned to the point for each direction
        int northY = y;
        int southY = y;
        int westX = x;
        int eastX = x;
        int neX = x;
        int neY = y;
        int swX = x;
        int swY = y;
        int nwX = x;
        int nwY = y;
        int seX = x;
        int seY = y;

        // While loops to check each direction***********
        
        // While loop for the north direction
        boolean northBool = true;
        // while loop that searches north in the board
        while (northBool) {
            wordNorth += myBoard[northY][tempX];
            if (wordNorth.length() > 3) {
                ifWordExistsInTST(wordNorth, valueCounter);
            }
            northY--;
            if (!north(tempX, northY)) {
                northBool = false;
            }

        }
        // While loop for the south direction
        boolean southBool = true;
        while (southBool) {
            wordSouth += myBoard[southY][tempX];
            if (wordSouth.length() > 3) {
                ifWordExistsInTST(wordSouth, valueCounter);
            }
            southY++;
            if (!south(tempX, southY)) {
                southBool = false;
            }
        }

        // While loop for the west direction
        boolean westBool = true;
        while (westBool) {
            wordWest += myBoard[westX][tempY];
            if (wordWest.length() > 3) {
                ifWordExistsInTST(wordWest, valueCounter);
            }
            westX--;
            if (!west(westX, tempY)) {
                westBool = false;
            }

        }

        // While loop for the east direction
        boolean eastBool = true;
        while (eastBool) {
            wordEast += myBoard[tempY][eastX];
            if (wordEast.length() > 3) {
                ifWordExistsInTST(wordEast, valueCounter);
            }
            eastX++;
            if (!east(eastX, tempY)) {
                eastBool = false;
            }
        }

        // While loop for the northeast direction
        boolean northEastBool = true;
        while (northEastBool) {
            wordNE += myBoard[neY][neX];
            if (wordNE.length() > 3) {
                ifWordExistsInTST(wordNE, valueCounter);
            }
            neX++;
            neY--;
            if (!north(neX, neY) || !east(neX, neY)) {
                northEastBool = false;
            }
        }

        // While loop for the southwest direction
        boolean southWestBool = true;
        while (southWestBool) {
            wordSW += myBoard[swY][swX];
            if (wordSW.length() > 3) {
                ifWordExistsInTST(wordSW, valueCounter);
            }
            swX--;
            swY++;
            if (!south(swX, swY) || !west(swX, swY)) {
                southWestBool = false;
            }
        }

        // While loop for the northwest direction
        boolean northWestBool = true;
        while (northWestBool) {
            wordNW += myBoard[nwY][nwX];
            if (wordNW.length() > 3) {
                ifWordExistsInTST(wordNW, valueCounter);
            }
            nwX--;
            nwY--;
            if (!north(nwX, nwY) || !west(nwX, nwY)) {
                northWestBool = false;
            }

        }

        // While loop for the southeast direction
        boolean southEastBool = true;
        while (southEastBool) {
            wordSE += myBoard[seY][seX];
            if (wordSE.length() > 3) {
                ifWordExistsInTST(wordSE, valueCounter);
            }
            seX++;
            seY++;
            if (!south(seX, seY) || !east(seX, seY)) {
                southEastBool = false;
            }
        }

    }

    // This method checks to see if words greater than 3 characters
    // exist in the dictionary
    // the word is passed in from the "checkTheBoard" method
    // and placed in the "foundWords" TST is it exists in the dictionary
    public static void ifWordExistsInTST(String word, int value) {
        word = word.toLowerCase();
        Iterable<String> wild = myTST.wildcardMatch(word);
        if (word.indexOf('.') >= 0) {
            for (String s : wild) {
                foundWords.put(s, value++);
            }
        } else {
            if (myTST.contains(word)) {
                foundWords.put(word, value++);
            }
        }
    }

    // This method is called from the "checkTheBoard" method and sees
    // if the board can be traversed further north
    public static boolean north(int x, int y) {
        if (y > 0) {
            return true;
        } else {
            return false;
        }
    }
    
    // This method is called from the "checkTheBoard" method and sees
    // if the board can be traversed further south
    public static boolean south(int x, int y) {
        if (y < dimension) {
            return true;
        } else {
            return false;
        }
    }
    
    // This method is called from the "checkTheBoard" method and sees
    // if the board can be traversed further east
    public static boolean east(int x, int y) {
        if (x < dimension) {
            return true;
        } else {
            return false;
        }
    }

    // This method is called from the "checkTheBoard" method and sees
    // if the board can be traversed further west
    public static boolean west(int x, int y) {
        if (x > 0) {
            return true;
        } else {
            return false;
        }
    }
}
