- 👋 Hi, I’m @priyankadey1993
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
priyankadey1993/priyankadey1993 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
#include <iostream>
#include<conio.h>
#include<dos.h>
#include<stdlib.h>
#include<string.h>
#include <windows.h>
#include <time.h>
#include<cmath>
#include<vector>
using namespace std;

HANDLE console = GetStdHandle(STD_OUTPUT_HANDLE);
COORD CursorPosition;

#define SCREEN_WIDTH 90
#define SCREEN_HEIGHT 26
#define TOTAL_OBSTACLE 15
   
class drawborder {
	public :
		
		void gotoxy(int x, int y) 
		{
			CursorPosition.X = x;
			CursorPosition.Y = y;
			SetConsoleCursorPosition(console, CursorPosition);
		}

		void setcursor(bool visible, DWORD size) 
		{
			if(size == 0)
				size = 20;

			CONSOLE_CURSOR_INFO lpCursor;
			lpCursor.bVisible = visible;
			lpCursor.dwSize = size;
			SetConsoleCursorInfo(console,&lpCursor);
		}
		
		void draw_border() 
		{
			for(int i=0; i<SCREEN_WIDTH; i++) 
			{
				gotoxy(i,0);
				cout<<"!";
				gotoxy(i,SCREEN_HEIGHT);
				cout<<"!";
			}

			for(int i=0; i<SCREEN_HEIGHT; i++) 
			{
				gotoxy(0,i);
				cout<<"!";
				gotoxy(SCREEN_WIDTH,i);
				cout<<"!";
			}
					 
		    gotoxy(SCREEN_WIDTH-1,SCREEN_HEIGHT-1);
		    cout<<"#";
		}
};


class my_object :virtual public drawborder  {
	protected:
		char obj[2][6] = { '/','-','-','o','\\',' ',
		                   '\\','_','_','_','/','>' };
		 
	public:
		COORD pos1;
		int objposY = 2;
		int objposX = 6;
		
		
		void drawobj()
		{
		   ;
		   
			for(int i=0; i<2; i++) {
				for(int j=0; j<6; j++) {
					gotoxy(j+objposX,i+objposY);
					cout<<obj[i][j];
				}
			}
		}
		
		void eraseobj() {
			for(int i=0; i<2; i++) {
				for(int j=0; j<6; j++) {
					gotoxy(j+objposX,i+objposY);
					cout<<" ";
				}
			}
		}
		
		int win()
		{
			for(int i=0; i<2; i++) 
			for(int j=0; j<6; j++) 
			if(objposX+j == SCREEN_WIDTH-1 && objposY+i == SCREEN_HEIGHT-1 )
			{
				return 1;
			}
				return 0;
			
		}
		
		int boundry_colide()
		{
			if(objposX >= SCREEN_WIDTH-1 || objposX <= 0 )
			{
				return 1;
			}
			else if(objposY >= SCREEN_HEIGHT-1||objposY <= 0)
			{
				return 1;
			}
			else
			{
				return 0;
			}
		}
	
};


class obstacle :virtual public drawborder,virtual protected my_object
 {

    
 	  
 	public:
 		COORD pos ;
 		char obstacle[4][4];
 	    static int count_obstacle;
 	    
 	void trigle()
 	{
 		int i=0,j=0;
        for(i = 1; i <= 4; i++)
  		{ 
    		for(int j = 1; j <= i; j++)
      			obstacle[i][j]='*';
      	
      		obstacle[i][j]='\n'; 
  		}  			
	}
	void rectangle()
	{
		int i,j;
    	int r= 4;
    	for (i = 0; i < r; i++)
		{
    		for (j = 0; j < r; j++) 
      			obstacle[i][j] = '*';
      			
   			obstacle[i][j] = '\n';
  		}
	}
	void hollow_rectangle()
	{
		int i,j;
		int m=3;
		int n=3;
		for (i = 0; i <= n; i++)
    {
        for (j = 0; j <= m; j++)
        {
            if (i == 0 || i == n ||
                j == 0 || j == m)        
               	obstacle[i][j] = '*';        
            else
                obstacle[i][j] = ' ';
        }
        	obstacle[i][j] = '\n';
    }
	}
	
	void genrate_pose()
	{
		pos.X = (rand()%(SCREEN_WIDTH-2)) ;
	    pos.Y = (rand()%(SCREEN_HEIGHT-2));
	}
	
	void drawobstacle() 
	{	
	   int k,l;
	   l:genrate_pose();
	   for(int i=0;i<2;i++)
	   for(int j=0;j<6;j++)
		if(pos.X==objposX+j || pos.Y==objposY+i)
	   {
	   		goto l;
	   }
	   
		if(count_obstacle<5)//TOTAL_OBSTACLE/2
       {
    			trigle();
				
       		count_obstacle++;
	   }
	   else if(count_obstacle<10)
	   {
	   		rectangle();
	   	     count_obstacle++;
       		
	   }
	   else
	   {
	   	for(int i=0; i<4; i++) 				
			for(int j=0; j<4; j++)
	   	     obstacle[i][j] = 0;
	   	     
	   	    hollow_rectangle();
	   	
	   }
	   
		for(int i=0; i<4; i++) 				
			for(int j=0; j<4; j++)
			{
				gotoxy(j+pos.X,i+pos.Y);
				cout<<obstacle[i][j];
			}
	}
	
	COORD getpos()
	{
		return pos;
	}
	
	void eraseobstacle() 
	{
		for(int i=0; i<4; i++) 
		{				
			for(int j=0; j<4; j++) 
			{
				gotoxy(j+pos.X,i+pos.Y);
				cout<<" ";
			}		
		}
	}
};


int obstacle::count_obstacle=0;

drawborder d; 


class play :virtual public my_object , obstacle
{
    static int obstacle_cnt;
	vector<COORD>::iterator it;

	vector<COORD> obs_pos_coll;
     
	public:
	
	void reset()
	{
		objposY = 2;
		objposX = 6;
		obstacle_cnt =0;
		count_obstacle =0;
		obs_pos_coll.clear();
	}
	
	int collide(COORD position)
	{   
	    if((objposX == position.X) && (objposY == position.Y ))
				{
					return 1;
				}
		for(int i=0,j=0;j<6,i<2;j++,i++) 				
		    if((j+objposX == position.X ) && (i+objposY == position.Y ))
				{
					return 1;
				}		
		for(int i=0,j=0; i<4,j<4;j++, i++) 				
		    if((objposX == j+position.X) && (objposY == i+position.Y ))
				{
					return 1;
				}
		
	    for(int i=0,j=0;i<6,j<4;j++,i++)	
			if((i+objposX==position.X)&&(objposY==position.Y+j))
				{
					return 1;
				}
		for(int i=0,j=0;i<2,j<4;j++,i++)	
			if((objposX==position.X+j)&&(objposY+i==position.Y))
				{
					return 1;
				}		
		/*
		for(int i=0,j=0;i<2,j<4;j++,i++)
		{
			if((objposX==position.X)&&(objposY==j+position.Y)||(objposY+i==position.Y))
			return 1;
		}	
		*/
		return 0;
	}
	
	int play1()
	{	
		system("cls");
		draw_border();	
		reset();

        while(1){
			if(kbhit())
			{
				char ch = getch();
				switch(ch)
				{
				case 'w':
					objposY-=1;
					break;
		    	case 's':
					objposY+=1;
					break;
				case 'a':	  
					objposX-=1;
					break;
				case 'd':
					objposX+=1;
					break;
				case 'W':
					objposY-=1;
					break;
		    	case 'S':
					objposY+=1;
					break;
				case 'A':	  
					objposX-=1;
					break;
				case 'D':
					objposX+=1;
					break;	
				default:
				    MessageBox(NULL, "WRONG KEY !", "YOU ENTER A WRONG KEY !!", MB_ICONINFORMATION);
	   	 			break;
		        }
			}
			
			drawobj();
			Sleep(500);
	        eraseobj();
              
            if(obstacle_cnt<TOTAL_OBSTACLE)
			{
				drawobstacle();   	
			    obs_pos_coll.push_back(getpos());
				obstacle_cnt++;
	    	} 
		
	   
		    for(int i=0;i<obstacle_cnt;i++)	
		    {
	        	if((collide(obs_pos_coll.at(i)))==1)
	        	{
	        	
	    			MessageBox(NULL, "OBSTACLE COLIDE !", "OUT !", MB_ICONINFORMATION);
	    			return 0;
		    	}
		    }
		    
	    	if(boundry_colide()==1)
	    	{
	    		eraseobj();
	    		MessageBox(NULL, "BOUNDRY COLIDE !", "OUT !!", MB_ICONINFORMATION);
				return 0;
				break;
			}
		    
			if(win()==1)
			{
				MessageBox(NULL, "win", "YOU WIN THE GAME !", MB_ICONINFORMATION);    
				break;
			}   
		
        }
        
        return 0;
	} 
	
	
};

int play::obstacle_cnt=0;
play p;
int main() 
{	
	d.setcursor(0,0);
	char ch;
	int a=1;
	srand( (unsigned)time(NULL));
	
	do {
		
		system("cls");
		d.gotoxy(15,5); cout<<" -------------------------- "; 
		d.gotoxy(15,6); cout<<" |     COLIDATION GAME      | "; 
		d.gotoxy(15,7); cout<<" --------------------------";
		d.gotoxy(15,9); cout<<"1. Start Game";
    	d.gotoxy(15,10); cout<<"Press any key";
		ch=getch();
		
		if(ch)
	    	a=p.play1();
	   // p.reset();
	}while(1);
	return 0;
}
