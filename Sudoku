package sudoku.game;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Random;
import javax.swing.BorderFactory;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JSeparator;
import javax.swing.JTextField;
import javax.swing.SwingConstants;
import javax.swing.border.Border;

public class Sudoku {
    public static void main(String[] args) {
        //Code
        setup();
    }

    public static void setup() {
        int n = 9;  //size if n*n grid
        int[][] pseudoboard = new int[n][n];
        int[][] boardsoln = new int[n][n];
        boolean built = false; //sudoku not build
        while(!built)
            built = buildsudoku(n,pseudoboard,boardsoln); //to build we need n,pseudoboard, boardsoln

        //Design an gui using java swing
        JFrame frame = new JFrame();
        frame.setSize(600, 600); //define size of frame
        frame.setTitle("Sudoku Game");  //title
        frame.setResizable(false); //cannot play in fullscreen
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(Color.black); //background color of panel
        JPanel board = new JPanel();
        board.setPreferredSize(new Dimension(500,500));
        board.setLayout(new GridLayout(n,n));
        JTextField[][] fieldref = new JTextField[n][n];

        for(int i=0;i<n;i++)
            for(int j=0;j<n;j++)
            {
                JTextField field = new JTextField();
                field.setHorizontalAlignment(JTextField.CENTER);
                if(pseudoboard[i][j]!=0)
                {
                    field.setText(Integer.toString(pseudoboard[i][j]));
                    field.setForeground(Color.black);
                    field.setFont(new Font("TIMES NEW ROMAN", Font.BOLD, 14));
                    field.setBackground(Color.lightGray);
                    field.setEditable(false);
                }
                if(((i+1)%(int)Math.sqrt(n))==0 && ((j+1)%(int)Math.sqrt(n))==0)
                    field.setBorder(BorderFactory.createMatteBorder(1, 1, 3, 3, Color.BLACK));
                else if(((i+1)%(int)Math.sqrt(n))==0)
                    field.setBorder(BorderFactory.createMatteBorder(1, 1, 3, 1, Color.BLACK));
                else if(((j+1)%(int)Math.sqrt(n))==0)
                    field.setBorder(BorderFactory.createMatteBorder(1, 1, 1, 3, Color.BLACK));
                else
                    field.setBorder(BorderFactory.createMatteBorder(1, 1, 1, 1, Color.BLACK));
                board.add(field);
                fieldref[i][j] = field;
            }

        //Design a control panel
        JPanel control = new JPanel(new FlowLayout());
        control.setBackground(Color.black);
        JButton reset = new JButton("Reset");

        reset.setFont(new Font("TIMES NEW ROMAN", Font.BOLD, 18));
        reset.setHorizontalAlignment(SwingConstants.CENTER);
        JButton submit = new JButton("Submit");

        submit.setFont(new Font("TIMES NEW ROMAN", Font.BOLD, 18));
        submit.setHorizontalAlignment(SwingConstants.CENTER);
        JButton solve = new JButton("Solve");

        solve.setFont(new Font("TIMES NEW ROMAN", Font.BOLD, 18));
        solve.setHorizontalAlignment(SwingConstants.CENTER);
        JButton newgame = new JButton("New Game");

        newgame.setFont(new Font("TIMES NEW ROMAN", Font.BOLD, 18));
        newgame.setHorizontalAlignment(SwingConstants.CENTER);
        control.add(reset);

        //for reset button remove all data with the help of actionPerformed() function
        reset.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                // TODO Auto-generated method stub
                for(int i=0;i<n;i++)
                    for(int j=0;j<n;j++)
                        if(pseudoboard[i][j]==0)
                            fieldref[i][j].setText("");
            }
        });

        control.add(submit);
        // submit all answer
        submit.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                // TODO Auto-generated method stub
                int[][] submission = new int[n][n];
                boolean allinp = true;//when user fill all spaces
                for(int i=0;i<n;i++)
                {
                    for(int j=0;j<n;j++)
                        if(fieldref[i][j].getText().isEmpty())
                        {
                            allinp = false;
                            break;
                        }
                    if(!allinp)
                        break;
                }
                if(!allinp) //when user not fill all input
                    JOptionPane.showMessageDialog(new JFrame(), "Fill all the boxes to submit.");
                else
                {
                    boolean rightinp = true; //check
                    for(int i=0;i<n;i++)
                    {
                        for(int j=0;j<n;j++)
                        {
                            try{
                                submission[i][j] = Integer.parseInt(fieldref[i][j].getText());
                                if(submission[i][j]<=0 || submission[i][j]>n)
                                {
                                    rightinp = false;
                                    break;
                                }
                            }
                            catch(Exception exp){
                                rightinp = false;
                                break;
                            }
                        }
                        if(!rightinp)
                            break;
                    }
                    if(!rightinp)//when all answer is wrong print this message
                        JOptionPane.showMessageDialog(new JFrame(), "WRONG INPUT");
                    else
                    {
                        boolean correct = true;
                        for(int i=0;i<n;i++)
                        {
                            for(int j=0;j<n;j++)
                            {
                                correct = issafe(n,submission,i,j);
                                if(!correct)
                                    break;
                            }
                            if(!correct)
                                break;
                        }
                        if(!correct) //when your answer are incorrect
                            JOptionPane.showMessageDialog(new JFrame(), "Wrong Answer. Try again !!");
                        else
                            JOptionPane.showMessageDialog(new JFrame(), "Right Answer. You Won !!");

                    }
                }
            }
        });

        control.add(solve);
        //check the solution of given sudoku
        solve.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                // TODO Auto-generated method stub
                for(int i=0;i<n;i++)
                    for(int j=0;j<n;j++)
                        fieldref[i][j].setText(Integer.toString(boardsoln[i][j]));
            }
        });

        control.add(newgame);
        //you want to play again or you skip this game want new game
        newgame.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                // TODO Auto-generated method stub
                frame.dispose();
                setup();
            }
        });

        panel.add(board, BorderLayout.NORTH);
        panel.add(control, BorderLayout.SOUTH);
        frame.add(panel);
        frame.setVisible(true);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }

    //function to build sudoku with the help of pseudoboard and board solution
    public static boolean buildsudoku(int n, int[][] pseudoboard, int[][] boardsoln)
    {
        //Sudoku with 17 clues has unique solution;
        Random random = new Random();
        int clues = 17;
        int p = 0;
        for(int i=0;i<n;i++)
            for(int j=0;j<n;j++)
                pseudoboard[i][j] = 0; //place is empty
        while(p<=clues)
        {
            int ri = random.nextInt(1000000000)%n;
            int ci = random.nextInt(1000000000)%n;
            int val = random.nextInt(1000000000)%(n+1);
            if(val==0 || pseudoboard[ri][ci]!=0)
                continue;
            pseudoboard[ri][ci] = val;
            boolean safe = issafe(n,pseudoboard,ri,ci);
            if(!safe)
            {
                pseudoboard[ri][ci] = 0;
                continue;
            }
            p++;
        }
        for(int i=0;i<n;i++)
            for(int j=0;j<n;j++)
                boardsoln[i][j] = pseudoboard[i][j];
        boolean solnexist = fillsudoku(n,boardsoln,0,0);
        if(solnexist)
            return true;
        else
            return false;
    }

    //function to fill empty spaces of sudoku
    public static boolean fillsudoku(int n, int[][] boardsoln, int ri, int ci)
    {
        if(ci==n) //Check if column value  becomes 9 , we move to next row and column start from 0
        {
            ri ++;
            ci = 0;
        }
        //if we have reached the 8th row and 9th column (0 indexed matrix)
        // we are returning true to avoid further backtracking
        if(ri==n && ci==0)
            return true;

        // Check if the current position of the grid already contains value >0
        // we iterate for next column
        if(boardsoln[ri][ci]!=0)
        {
            boolean retval = fillsudoku(n,boardsoln,ri,ci+1);
            return retval;
        }

        // Check if it is safe to place the num (1-9)  in the given row ,col ->we move to next column
        for(int i=1;i<=n;i++)
        {
            boardsoln[ri][ci] = i;
            boolean safe = issafe(n,boardsoln,ri,ci);
            if(!safe)
            {
                //removing the assigned num, since our assumption was wrong
                // and we go for next assumption with diff num value
                boardsoln[ri][ci] = 0;
                continue;
            }
            boolean retval = fillsudoku(n,boardsoln,ri,ci+1);
            if(!retval)
            {
                boardsoln[ri][ci] = 0;
                continue;
            }
            else
                return true;
        }
        return false;
    }

    //function to check whether we place number on given row and column or not
    public static boolean issafe(int n, int[][] pseudoboard, int ri, int ci)
    {
        boolean[] check = new boolean[n];
        for(int i=0;i<n;i++)
            check[i] = false;
        for(int i=0;i<n;i++) // Check if we find the same num in the similar row , we return false
        {
            if(pseudoboard[ri][i]==0)
                continue;
            if(!check[pseudoboard[ri][i]-1])
                check[pseudoboard[ri][i]-1] = true;
            else
                return false;
        }
        for(int i=0;i<n;i++)
            check[i] = false;
        for(int i=0;i<n;i++) //Check if we find the same num in the similar column , we return false
        {
            if(pseudoboard[i][ci]==0)
                continue;
            if(!check[pseudoboard[i][ci]-1])
                check[pseudoboard[i][ci]-1] = true;
            else
                return false;
        }
        int sn = (int)Math.sqrt(n); //square of 9 is 3
        int boxri = ri/sn;
        int boxci = ci/sn;
        for(int i=0;i<n;i++)
            check[i] = false;
        for(int i=0;i<sn;i++)   // Check if we find the same num in the particular 3*3 matrix, we return false
            for(int j=0;j<sn;j++)
            {
                if(pseudoboard[boxri*sn+i][boxci*sn+j]==0)
                    continue;
                if(!check[pseudoboard[boxri*sn+i][boxci*sn+j]-1])
                    check[pseudoboard[boxri*sn+i][boxci*sn+j]-1] = true;
                else
                    return false;
            }
        return true;
    }
}

