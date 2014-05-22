Black-Tile
==========

My first Java game (clone of Don't Step On the White Tile)

/**
 * @(#)BlackTile.java
 *
 *
 * @author Lavid Jaubs
 * @version 1.00 2014/5/8
 */

import java.awt.*;
import java.awt.event.*;
import java.util.*;

public class BlackTile extends java.applet.Applet
{
	Graphics g = getGraphics();
	
	Rectangle columnOne, columnTwo, columnThree, columnFour, restart;
	
	int score;
	boolean startScreen = true;
	boolean beginning = true;
	boolean gameOver = false;
	boolean win = false;
	boolean lose = false;
	double time = 0.0;	
	boolean[][] isBlack = new boolean[4][4];
	
	
	javax.swing.Timer t = new javax.swing.Timer(10, new ActionListener()
	{
		public void actionPerformed(ActionEvent e)
		{
			time += 0.01;
		}
	});
	
	public void init() 
	{
		score = 0;
		columnOne = new Rectangle(295,95,105,625);
		columnTwo = new Rectangle(400,95,105,625); 
		columnThree = new Rectangle(505,95,105,625);
		columnFour = new Rectangle(610,95,110,625);
		restart = new Rectangle(460,465,130,55);
		
		for(int r = 0; r < 4; r++)
			for(int c = 0; c < 4; c++)
				isBlack[r][c] = false;
				
		int rand;
		
		for(int row = 0; row < 4; row++)
		{
			rand = (int)Math.floor(Math.random()*4);
			isBlack[row][rand]=true;
		}
	}

	public void paint(Graphics g) 
	{
		if(startScreen)
		{
			g.setColor(Color.ORANGE);
			g.fillRect(0,0,1045,1000);
			
			g.setColor(Color.BLACK);
			g.setFont(new Font("Arial", Font.BOLD, 70));
			g.drawString("Welcome to Black Tile",200,300);
			
		}
		
		else
		{
			drawBoard(g);
			
			g.setColor(Color.RED);
			g.setFont(new Font("Arial", Font.BOLD, 80));
			g.drawString("" + score,80,90);
			
			finish(g);
		}
		
	}
	
	public void drawBoard(Graphics g)
	{
		g.setColor(Color.BLACK);
		
		for(int x = 295; x <= 715; x+= 105)	//vertical lines
		{
			g.fillRect(x,95,5,625);
		}
		
		for(int y = 95; y <= 715; y+= 155)		//horizontal lines
		{
			g.fillRect(295,y,425,5);
		}
		
		for(int r = 0; r < 4; r++)			//colors correct tile gray
		{
			for(int c = 0; c < 4; c++)
			{
				if(isBlack[r][c])
				{
					g.setColor(Color.GRAY);
					g.fillRect(300 + (105 * c), 100 + (155 * r), 100, 150);
				}
			}
		}
	}
	
	public void finish(Graphics g)
	{
		
		if(lose)
		{
			t.stop();
			
			gameOver = true;
			
			g.setColor(new Color(179,33,44));	//big white rectangle
			g.fillRect(0,0,1045,1000);
			
			/////final score/////
			
			g.setColor(Color.BLACK);	//outline
			g.fillRect(295,255,455,110);
			g.setColor(Color.WHITE);	//fill
			g.fillRect(300,260,445,100);
			
			g.setColor(Color.BLACK);
			g.setFont(new Font("Arial", Font.BOLD, 40));
			g.drawString("GAME OVER", 400, 300);
			g.drawString("Final Score: " + score, 385, 345);
			
			/////restart button/////
			
			g.setColor(Color.ORANGE);
			g.fillRect(460,465,130,55);
			
			g.setColor(Color.WHITE);
			g.drawLine(462,467,462,517);
			g.drawLine(463,467,463,517);
			
			g.setColor(Color.BLACK);
			g.drawLine(462,517,587,517);
			g.drawLine(462,516,587,516);
			
			g.drawLine(586,467,586,517);
			g.drawLine(587,467,587,517);
			
			g.setColor(Color.WHITE);
			g.drawLine(463,467,587,467);
			g.drawLine(463,468,587,468);
			
			g.setColor(Color.BLACK);
			g.setFont(new Font("Arial", Font.BOLD, 25));
			g.drawString("Restart", 480, 500);
		}

		if(win)
		{
			t.stop();
			
			gameOver = true;
			
			g.setColor(new Color(140,198,62));	//big white rectangle
			g.fillRect(0,0,1045,1000);
			
			g.setColor(Color.BLACK);	//outline
			g.fillRect(295,255,455,110);
			g.setColor(Color.WHITE);//fill
			g.fillRect(300,260,445,100);
			
			g.setColor(Color.BLACK);
			g.setFont(new Font("Arial", Font.BOLD, 40));
			g.drawString("YOU WIN", 430, 300);
			
			time = time * 100;
			time = Math.floor(time);
			time = time / 100;
			
			g.drawString("Your Time: " + time, 385, 345);
			
			/////restart button/////
			
			g.setColor(Color.ORANGE);
			g.fillRect(460,465,130,55);
			
			g.setColor(Color.WHITE);
			g.drawLine(462,467,462,517);
			g.drawLine(463,467,463,517);
			
			g.setColor(Color.BLACK);
			g.drawLine(462,517,587,517);
			g.drawLine(462,516,587,516);
			
			g.drawLine(586,467,586,517);
			g.drawLine(587,467,587,517);
			
			g.setColor(Color.WHITE);
			g.drawLine(463,467,587,467);
			g.drawLine(463,468,587,468);
			
			g.setColor(Color.BLACK);
			g.setFont(new Font("Arial", Font.BOLD, 25));
			g.drawString("Restart", 480, 500);
		}
	}
	
	public void shift()
	{
		repaint();
		for(int r = 3; r >= 0; r--)
		{
			if(r == 0)
			{
				for(int c = 0; c < 4; c++)
				{
					isBlack[r][c] = false;
				}
				int temp = (int)Math.floor(Math.random()*4);
				isBlack[r][temp] = true;
			}
			
			else
			{
				for(int c = 0; c < 4; c++)
				{
					isBlack[r][c] = isBlack [r-1][c];
				}
			}
		}
	}
	
	public void restart()
	{
		int rand;
		beginning = true;
		gameOver = false;
		lose = false;
		win = false;
		score = 0;
		time = 0.0;
		init();
		repaint();
	}

	public boolean mouseDown(Event e, int x, int y)
	{
		if(startScreen)
		{
			startScreen = false;
			repaint();
			return true;
		}
		
		if(beginning == true)
		{
			t.start();
			beginning = false;
		}
		
		if(gameOver == false)
		{
			if(columnOne.inside(x,y))
			{
				if(isBlack[3][0] == true)
				{
					score++;
					shift();
				}
				else
					lose = true;
				repaint();
			}
			
			else if(columnTwo.inside(x,y))
			{
				if(isBlack[3][1] == true)
				{
					score++;
					shift();
				}
				else
					lose = true;
				repaint();
			}
			
			else if(columnThree.inside(x,y))
			{
				if(isBlack[3][2] == true)
				{
					score++;
					shift();
				}
				else
					lose = true;
				repaint();
			}
			
			else if(columnFour.inside(x,y))
			{
				if(isBlack[3][3] == true)
				{
					score++;
					shift();
				}
				else
					lose = true;
				repaint();
			}
			
			if(score == 20)
			{
				win = true;
			}
		}
		
		if(gameOver)
		{
			if(restart.inside(x,y))
				restart();
		}

		return true;
		
	}
	
	public boolean keyDown(Event e, int key)
	{
		char letter = (char)key;
		
		startScreen = false;
		
		if(beginning == true && startScreen == false)
		{
			t.start();
			beginning = false;
		}
		
		if(gameOver == false)
		{
			if(letter == 'q')
			{
				if(isBlack[3][0] == true)
				{
					score++;
					shift();
				}
				else
					lose = true;
				repaint();
			}
			
			else if(letter == 'w')
			{
				if(isBlack[3][1] == true)
				{
					score++;
					shift();
				}
				else
					lose = true;
				repaint();
			}
			
			else if(letter == 'e')
			{
				if(isBlack[3][2] == true)
				{
					score++;
					shift();
				}
				else
					lose = true;
				repaint();
			}
			
			else if(letter == 'r')
			{
				if(isBlack[3][3] == true)
				{
					score++;
					shift();
				}
				else
					lose = true;
				repaint();
			}
			
			if(score == 20)
			{
				win = true;
			}
			
			
		}
		
		if(letter == ' ')
			restart();
		return true;
	}
    
}
