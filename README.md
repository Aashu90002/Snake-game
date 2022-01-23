Introduction

We have created an “Snake Game” using java programming language.
In this project we have used  three class i.e
•	SnakeGame
•	GameFrame
•	GamePanel
In class GamePanel we have  given all action of the game.
We have used various functions of the java language such a Jpanel which is used to implement all the action of game .we also used  Mykeyadpter to control the direction of the Snake by using Keyboard. We have also used delay function to control the speed of the Snake.

Then we have initialize the axis of the apple i.e appleX and appleY. We have the background of colour black. We can change it for our convenient. In project, we have use switch case to give direction of the snake .
‘L’=Left ,  ‘R’=Right , ‘U’=Up,  ‘D’=Down

Then if the snake collide with the boundary then it will show game over and also if snake head collide with the body
 
 




Code/Program:

//Class =SnakeGame.java//
public class SnakeGame {

	public static void main(String[] args) {
		new GameFrame ();

	}
}



//Class= GameFrame.java//

import javax.swing.JFrame;

public class GameFrame extends JFrame{
	
    GameFrame(){
    this.add(new GamePanel());
     this.setTitle("Snake");
    this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    this.setResizable(false);
     this.pack();
     this.setVisible(true);;
     this.setLocationRelativeTo(null);
  }
}

//Class= GamePanel.java//
public class GamePanel extends JPanel implements ActionListener {
     static final int SCREEN_WIDTH = 600;
     static final int SCREEN_HEIGHT = 600;
     static final int UNIT_SIZE = 25;
     static final int GAME_UNITS = (SCREEN_WIDTH*SCREEN_HEIGHT)/UNIT_SIZE;
     static final int DELAY = 70;
     final int x[] = new int[GAME_UNITS];
     final int y[] = new int[GAME_UNITS];
     int bodyParts = 6;
     int applesEaten;
     int appleX;
     int appleY;
     char direction = 'R';
     boolean running = false;
     Timer timer;
   Random random;
    GamePanel(){
          random = new Random();
            this.setPreferredSize(new Dimension(SCREEN_WIDTH,SCREEN_HEIGHT));
            this.setBackground(Color.black);
           this.setFocusable(true);
       this.addKeyListener(new MyKeyAdapter());
       
          startGame();
       }
     public void startGame() {
    newApple();
   running = true;
     timer = new Timer (DELAY,this);
     timer.restart();
    }
      public void paintComponent(Graphics g) {
      super.paintComponent(g);
      draw(g);
   }
           public void draw(Graphics g) {
               if(running) {
         g.setColor(Color.red);
              g.fillOval(appleX, appleY, UNIT_SIZE, UNIT_SIZE);
        for(int i = 0; i< bodyParts ; i++) {
        if(i == 0) {
             g.setColor(Color.green);

            g.fillRect(x[i], y[i], UNIT_SIZE, UNIT_SIZE);
         }
            else {
         g.setColor(new Color(45,180,0));
          g.fillRect(x[i], y[i], UNIT_SIZE, UNIT_SIZE);
          }
        }
         g.setColor(Color.red);
         //coloure of score/
         g.setFont( new Font("Ink Free",Font.BOLD, 40));
           FontMetrics metrics = getFontMetrics(g.getFont());
      g.drawString("Score:"+applesEaten, (SCREEN_WIDTH -metrics.stringWidth("Score:" +applesEaten))/2, g.getFont().getSize());
     }
        else {
     gameOver(g);
}
}
    public void newApple() {
        appleX = random.nextInt((int)(SCREEN_WIDTH/UNIT_SIZE))*UNIT_SIZE;
        appleY = random.nextInt((int)(SCREEN_HEIGHT/UNIT_SIZE))*UNIT_SIZE;
         }
           public void move() {
          for(int i = bodyParts;i>0;i--) {
                 x[i] = x[i-1];
                  y[i] = y[i-1];
           }
            switch(direction) {
            case 'U' :
            y[0] = y[0] - UNIT_SIZE;
              break;
                case 'D':
              y[0] = y[0] + UNIT_SIZE;
                break;
                 case 'L':
                 x[0] = x[0] - UNIT_SIZE;
                      break;
                   case 'R':
                 x[0] = x[0] + UNIT_SIZE;
                   break;
                    }
                }
                public void checkApple() {
              if((x[0] == appleX) &&(y[0] == appleY)) {
               bodyParts++;
             applesEaten++;
                 newApple();
                }
              }
           public void checkCollisions() {
           //checks if head collides with body
            for(int i = bodyParts;i>0;i--) {
           if((x[0] == x[i] && (y[0] == y[i]))){

           running = false;
             }
            }
            
//check if head touches left boreder
           if(x[0] < 0) {
          running = false;
        }
//check if head touches right border
           if(x[0] > SCREEN_WIDTH) {
       running = false;
         }
//check if head touches top border
         if(y[0] < 0) {
           running = false;
         }
//check if head touches bottom border
       if(y[0] > SCREEN_HEIGHT) {
               running = false;
            }
            if(!running) {
        timer.stop();
          }
           }
             public void gameOver(Graphics g) {
            	 
      //Score
      g.setColor(Color.green);
           g.setFont( new Font("Ink Free",Font.BOLD, 40));
            FontMetrics metrics1 = getFontMetrics(g.getFont());
           g.drawString("Score:"+applesEaten, (SCREEN_WIDTH -metrics1.stringWidth("Score:" +applesEaten))/2, g.getFont().getSize());

//Game Over text
            g.setColor(Color.blue);
        g.setFont( new Font("Ink Free",Font.BOLD, 75));
          FontMetrics metrics2 = getFontMetrics(g.getFont());
           g.drawString("Game Over", (SCREEN_WIDTH - metrics2.stringWidth("GameOver"))/2, SCREEN_HEIGHT/2);
}
@Override
      public void actionPerformed(ActionEvent e) {
            if(running) {
            move();
      checkApple();
     checkCollisions();
        }
         repaint();
         }
             public class MyKeyAdapter extends KeyAdapter{
        @Override
        public void keyPressed(KeyEvent e) {
           switch(e.getKeyCode()) {
         case KeyEvent.VK_LEFT:

            if(direction != 'R') {
          direction = 'L';
         }
           break;
         case KeyEvent.VK_RIGHT:
          if(direction != 'L') {
   direction = 'R';
      }
     break;
         case KeyEvent.VK_UP:
      if(direction != 'D') {
       direction = 'U';
         }
          break;
                case KeyEvent.VK_DOWN:
                 if(direction != 'U') {
   direction = 'D';
         }
        break;
          }
         }
     }
            }

