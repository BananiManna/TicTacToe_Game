# TicTacToe_Game
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Random;
import javax.swing.*;

public class TicTacToe implements ActionListener {
    Random rand = new Random();
    JFrame frame = new JFrame();
    JPanel titlePanel = new JPanel();
    JPanel buttonPanel = new JPanel();
    JLabel text = new JLabel();
    JButton[] button = new JButton[9];
    boolean player1Turn;
    boolean player2Turn;
    int moves = 0;
    int xScore = 0;
    int oScore = 0;
    public TicTacToe() {
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(1200, 600);
        frame.getContentPane().setBackground(new Color(50, 0, 50));
        frame.setLayout(new BorderLayout());
        frame.setVisible(true);

        text.setBackground(new Color(20, 20, 20));
        text.setForeground(new Color(25, 255, 0));
        text.setFont(new Font("Ink Free", Font.BOLD, 80));
        text.setHorizontalAlignment(JLabel.CENTER);
        text.setText("Tic-Tac-Toe");
        text.setOpaque(true);

        titlePanel.setLayout(new BorderLayout());
        titlePanel.setBounds(0, 0, 800, 100);
        buttonPanel.setLayout(new GridLayout(3, 3));
        buttonPanel.setBackground(new Color(50, 250, 250));

        for (int i = 0; i < 9; i++) {
            button[i] = new JButton();
            buttonPanel.add(button[i]);
            button[i].setFont(new Font("MV Boli", Font.BOLD, 120));
            button[i].setFocusable(false);
            button[i].addActionListener(this);
        }

        titlePanel.add(text);
        frame.add(titlePanel, BorderLayout.NORTH);
        frame.add(buttonPanel, BorderLayout.CENTER);

        firstTurn();
    }

    public void actionPerformed(ActionEvent e) {
        for (int i = 0; i < 9; i++) {
            if (e.getSource() == button[i] && button[i].getText().equals("")) {
                if (player1Turn) {
                    button[i].setForeground(new Color(255, 0, 0));
                    button[i].setText("X");
                    player1Turn = false;
                    player2Turn = true;
                    text.setText("O turn");
                } else if (player2Turn) {
                    button[i].setForeground(new Color(0, 0, 255));
                    button[i].setText("O");
                    player2Turn = false;
                    player1Turn = true;
                    text.setText("X turn");
                }
                moves++;
                check();
            }
        }
    }

    public void firstTurn() {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        if (rand.nextInt(2) == 0) {
            player1Turn = true;
            text.setText("X Turn");
        } else {
            player2Turn = true;
            text.setText("O Turn");
        }
    }

    public void check() {
        // Check X win condition
        if ((button[0].getText().equals("X") && button[1].getText().equals("X") && button[2].getText().equals("X")) ||
                (button[3].getText().equals("X") && button[4].getText().equals("X") && button[5].getText().equals("X")) ||
                (button[6].getText().equals("X") && button[7].getText().equals("X") && button[8].getText().equals("X")) ||
                (button[0].getText().equals("X") && button[3].getText().equals("X") && button[6].getText().equals("X")) ||
                (button[1].getText().equals("X") && button[4].getText().equals("X") && button[7].getText().equals("X")) ||
                (button[2].getText().equals("X") && button[5].getText().equals("X") && button[8].getText().equals("X")) ||
                (button[0].getText().equals("X") && button[4].getText().equals("X") && button[8].getText().equals("X")) ||
                (button[2].getText().equals("X") && button[4].getText().equals("X") && button[6].getText().equals("X"))) {
            xWins();
        }

        // Check O win condition
        else if ((button[0].getText().equals("O") && button[1].getText().equals("O") && button[2].getText().equals("O")) ||
                (button[3].getText().equals("O") && button[4].getText().equals("O") && button[5].getText().equals("O")) ||
                (button[6].getText().equals("O") && button[7].getText().equals("O") && button[8].getText().equals("O")) ||
                (button[0].getText().equals("O") && button[3].getText().equals("O") && button[6].getText().equals("O")) ||
                (button[1].getText().equals("O") && button[4].getText().equals("O") && button[7].getText().equals("O")) ||
                (button[2].getText().equals("O") && button[5].getText().equals("O") && button[8].getText().equals("O")) ||
                (button[0].getText().equals("O") && button[4].getText().equals("O") && button[8].getText().equals("O")) ||
                (button[2].getText().equals("O") && button[4].getText().equals("O") && button[6].getText().equals("O"))) {
            oWins();
        }

        // Check for a tie
        else if (moves == 9) {
            tie();
        }
    }

    public void xWins() {
        highlightWinningRow();
        disableAllButtons();
        xScore++;
        text.setText("X wins || X score: " + xScore + " |  O score: " + oScore);
        askPlayAgain();
    }

    public void oWins() {
        highlightWinningRow();
        disableAllButtons();
        oScore++;
        text.setText("O wins || X score: " + xScore + " |  O score: " + oScore);
        askPlayAgain();
    }

    private void tie() {
        disableAllButtons();
        text.setText("It's a Tie!");
        askPlayAgain();
    }

    private void disableAllButtons() {
        for (int i = 0; i < 9; i++) {
            button[i].setEnabled(false);
        }
    }

    private void highlightWinningRow() {
        for (int i = 0; i < 9; i++) {
            button[i].setBackground(new Color(255, 75, 0));
        }
    }

    private void askPlayAgain() {
        int option = JOptionPane.showConfirmDialog(frame, "Do you want to play again?", "Play Again", JOptionPane.YES_NO_OPTION);
        if (option == JOptionPane.YES_OPTION) {
            resetGame();
        } else {
            System.exit(0);
        }
    }

    private void resetGame() {
        for (int i = 0; i < 9; i++) {
            button[i].setText("");
            button[i].setEnabled(true);
            button[i].setBackground(null);
        }
        moves = 0;
        firstTurn();
    }

    public static void main(String[] args) {
        new TicTacToe();
    }
}
