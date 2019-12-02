# GraphicsLab1

/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package javaapplication1;
//package cse423_lab02;
/**
 *
 * @author Nazmuddin Al Masood - nazmumasood96@gmail.com - 16101272
 */
import com.jogamp.opengl.GL2;
import com.jogamp.opengl.GLAutoDrawable;
import com.jogamp.opengl.GLCapabilities;
import com.jogamp.opengl.GLEventListener;
import com.jogamp.opengl.GLProfile;
import com.jogamp.opengl.awt.GLCanvas;
import com.jogamp.opengl.glu.GLU;

import java.awt.event.WindowEvent;
import java.lang.Math;
import javax.swing.JFrame;

class ThirdGLEventListener implements GLEventListener {
    /**
     * Interface to the GLU library.
     */
    private GLU glu; private GL2 gl;

    private void doIt(GL2 gl){
        /*gl.glColor3d(1,0.5, 0.7);
        gl.glPointSize(10.0f);
        gl.glBegin(GL2.GL_POINTS);
        gl.glVertex2d(10, -50);
        gl.glEnd();*/

        //Drawing the axes
        //BruteForceDrawLine(gl, -100, 0, 100, 0);
        //DDA_Algo(gl, 0, -100, 0, 100);

        //BruteForceDrawLine(gl, -100, -50, -50, 180);
        //DDA_Algo(gl, -100, -50, -50, 180);

        //MidpointLineForZone0(gl, 100, 50, 200, 80);
        //MidpointLineForAnyZone(50, -100, 100, -190);

        //MidpointCircleForZone1Arc(50,-50, 100);
        //MidpointCircleForAllZoneArcs(-5, 8,  100);
        //MidpointLineForAnyZone(50,60, 50+100/Math.sqrt(2), 60+100/Math.sqrt(2));
        //DDA_Algo(gl,50, 60, 50, 160);

        //My initials draw
//        //N
//        DDA_Algo(gl, -250,-50, -250, 50);
//        MidpointLineForAnyZone(-151, -50, -250, 50);
//        DDA_Algo(gl, -150,-50, -150, 50);
//        //A
//        MidpointLineForAnyZone(-50, -50, 0, 50);
//        MidpointLineForAnyZone(50, -50, 0, 50);
//        DDA_Algo(gl, -25,0, 25, 0);
//        //M
//        DDA_Algo(gl, 250,-50, 250, 50);
//        DDA_Algo(gl, 150,-50, 150, 50);
//        MidpointLineForAnyZone(150, 50, 200, 10);
//        MidpointLineForAnyZone(250, 50, 200, 10);

          //polygonDrawing(50);
          //polyNew(gl, 100, 100);
          
          //Brac logo try
          MidpointCircleForAllZoneArcs(0, 0,  210);
          
          gl.glColor3d(0, 0, 1);
          MidpointCircleForZone1Arc(-150,-200, 200);
          MidpointCircleForZone2Arc(150,-200, 200);
          
          gl.glColor3d(0, 0, 0);
          MidpointCircleForZone1Arc(-150,-240, 200);
          MidpointCircleForZone2Arc(150,-240, 200);
          
          gl.glColor3d(0.0, 1.0, 1.0);
          MidpointCircleForZone1Arc(-150,-280, 200);
          MidpointCircleForZone2Arc(150,-280, 200);
        
        //Brazil flag try
        //polygonDrawing(4, 300);
//        DDA_Algo(gl, -350,-250, -350, 250);
//        DDA_Algo(gl, 350,-250, 350, 250);
//        DDA_Algo(gl, -350,-250, 350, -250);
//        DDA_Algo(gl, -350,250, 350, 250);
//        polyNew(gl, 2, 150);
//        MidpointCircleForAllZoneArcs(0, 0,  70);
        
    }
    
    //-------strange code-----------
    //newPolyTry
    void polyNew(GL2 gl, double n, double r){
        double rad = 360/n;
        double newRad = 0;
        double x1 = r * Math.cos(newRad * 0.0174);
        double y1 = r * Math.sin(newRad * 0.0174);
        for(int i=0; i<=n;i++){
            newRad += rad;
            double x2 = r * Math.cos(newRad * 0.0174);
            double y2 = r * Math.sin(newRad * 0.0174);
            midPoint(gl, x1,y1,x2,y2);
            x1 = x2; y1 = y2;
        }
    }
    //circle
    void MidpointCircle(GL2 gl,int cX, int cY, int r){
        int x = cX; 
        int y = r+cY;
        int d = 5 - 4*r;
        //midPoint(gl,x,y);
    }
    //mid point
    public void midPoint(GL2 gl, double x1, double y1, double x2, double y2){
        double zone = findZone(x1,y1,x2,y2);
        System.out.print("zone = "+zone);
        if(zone == 0) midPoitnLineDrawing(gl,x1, y1, x2, y2,zone);
        else if (zone == 1) midPoitnLineDrawing(gl,y1,x1, y2,x2,zone);
        else if (zone == 2) midPoitnLineDrawing(gl,y1,-x1, y2,-x2,zone);
        else if (zone == 3) midPoitnLineDrawing(gl,-x1,y1,-x2,y2,zone);
        else if (zone == 4) midPoitnLineDrawing(gl,-x1,-y1,-x2,-y2,zone);
        else if (zone == 5) midPoitnLineDrawing(gl,-y1,-x1, -y2,-x2,zone);
        else if (zone == 6) midPoitnLineDrawing(gl,-y1,x1, -y2,x2,zone);
        else  midPoitnLineDrawing(gl,x1,-y1,x2, -y2,zone);
    }
    // find zone 
    public double findZone(double x1, double y1, double x2, double y2){
        double dx = x2 - x1;
        double dy = y2 - y1;
        if (dx >= 0 && dy >= 0){
            if(Math.abs(dx) >= Math.abs(dy)) return 0;
            else return 1;
        }else if (dx < 0 && dy >=0){
            if(Math.abs(dx) >= Math.abs(dy)) return 3;
            else return 2;
        }else if (dx <0 && dy <0){
            if(Math.abs(dx) >= Math.abs(dy)) return 4;
            else return 5;
        }else{
            if(Math.abs(dx) >= Math.abs(dy)) return 7;
            else return 6;
        }
    }
    //draw algo
    public void drawPixle(GL2 gl,double x, double y){
        gl.glBegin(GL2.GL_POINTS); 
        gl.glColor3d(1.0, 1.0, 0);
        gl.glPointSize(6.0f);
        gl.glVertex2d(x, y);
        gl.glEnd();
    }
    // mid point algo 
    public void midPoitnLineDrawing(GL2 gl,double x1, double y1, double x2, double y2, double zone){
        double dx = x2 - x1;
        double dy = y2 - y1;
        double d = 2*dy-dx;
        double dE = 2*dy;
        double dNE = 2*(dy - dx);
        double x = x1, y = y1;
        while(x <= x2){
            if(zone == 1){
                drawPixle(gl,y,x);
            }else if(zone == 2){
                drawPixle(gl,-y,x);
            }else if(zone == 3){
                drawPixle(gl,-x,y);
            }else if(zone == 4){
                drawPixle(gl,-x,-y);
            }else if(zone == 5){
                drawPixle(gl,-y,-x);
            }else if(zone == 6){
                drawPixle(gl,y,-x);
            }else if(zone == 7){
                drawPixle(gl,x,-y);
            }else if(zone == 0){
                drawPixle(gl,x,y);
            }
            if(d <= 0){
                d += dE;
            }else{
                y++;
                d+=dNE;
            }
            x++;
        }
    }
    //--------strange code----------

    //Polygon drawing algo
    private void polygonDrawing(int n, double r1){
        double r = r1; double theta = 360/n;//((n-2)*180.0)/n
        double [] x = new double[n];
        double [] y = new double[n];
       
        double degree = 45.0;
        for(int i=0; i<n; i++){
            x[i] = r * Math.cos(Math.toRadians(degree));
            y[i] = r * Math.sin(Math.toRadians(degree));
            degree = degree + theta;
        }
       
        System.out.println(n);
        for(int i=0; i<n; i++){
            System.out.println(x[i]+", "+y[i]);
        }
       
        for(int i=0; i<n; i++){
            if(i<n-1){
                MidpointLineForAnyZone(x[i], y[i], x[i+1], y[i+1]);
            }
            else{
                MidpointLineForAnyZone(x[0], y[0], x[i], y[i]);
            }
        }
    }
   
    //MidpointCircleDrawing_Algo irrespective of zones
    private void MidpointCircleForAllZoneArcs(double x1, double y1, double r){
        MidpointCircleForZone1ArcButModified( x1, y1, r);
    }
    private void MidpointCircleForZone1ArcButModified(double x_center, double y_center, double r){
        double d= 3-2*r;//1-r;
        double dE = 3;
        double dSE = 5 - 2*r;
        double x=0, y=r;

        while (y>=x){

            if (d<0){ //dE
                d = d + dE;
                dE += 2;
                dSE +=2;
            }

            else { //dSE
                d = d + dSE;
                dE += 2;
                dSE +=4;
                y--;
            }

            x++;

            draw8Way(x, y, x_center, y_center);
        }// while
    }
    private void draw8Way(double x, double y, double xc, double yc) {
        gl.glBegin(GL2.GL_POINTS);
        gl.glColor3d(0, 0, 1); 
        
        gl.glVertex2d(x+xc, y+yc);
        gl.glVertex2d(y+xc, x+yc);

        gl.glVertex2d(-x+xc, y+yc);
        gl.glVertex2d(-y+xc, x+yc);

        //gl.glVertex2d(-x+xc, -y+yc);//comment out this for brac
        gl.glVertex2d(-y+xc, -x+yc);


       // gl.glVertex2d(x+xc, -y+yc);//comment out this for brac
        gl.glVertex2d(y+xc, -x+yc);

        gl.glEnd();
    }
    
    //MidpointCircleDrawing_Algo  [ pixel draw starts clock-wise from (0,r) ]
    private void MidpointCircleForZone2Arc(double x_center, double y_center, double r){
        double d= 1-r;
        double dE = 3;
        double dSE = 5 - 2*r;
        double x=0, y=r;
        gl.glBegin(GL2.GL_POINTS);

        while (y>=x){
            //gl.glVertex2d(x+x_center, y+y_center);
            gl.glVertex2d(-x+x_center, y+y_center);;
            if (d<0){ //dE
                d = d + dE;
                dE += 2;
                dSE +=2;
            }

            else { //dSE
                d = d + dSE;
                dE += 2;
                dSE +=4;
                y--;
            }

            x++;
        }// while
        gl.glEnd();

    }

    //MidpointCircleDrawing_Algo  [ pixel draw starts clock-wise from (0,r) ]
    private void MidpointCircleForZone1Arc(double x_center, double y_center, double r){
        double d= 1-r;
        double dE = 3;
        double dSE = 5 - 2*r;
        double x=0, y=r;
        gl.glBegin(GL2.GL_POINTS);

        while (y>=x){
            gl.glVertex2d(x+x_center, y+y_center);

            if (d<0){ //dE
                d = d + dE;
                dE += 2;
                dSE +=2;
            }

            else { //dSE
                d = d + dSE;
                dE += 2;
                dSE +=4;
                y--;
            }

            x++;
        }// while
        gl.glEnd();

    }

    //MidpointLineDrawing_Algo irrespective of zones
    private void MidpointLineForAnyZone(double x1, double y1, double x2, double y2) {
        int zone = getZone(x1, y1, x2, y2);
        if (zone == -1){return;}
        switch (zone){
            //encoding the zones to zone 0
            case 0:
                MidpointLineForZone0ButModified(0, x1, y1, x2, y2);
                break;
            case 1:
                MidpointLineForZone0ButModified(1, y1, x1, y2, x2);
                break;
            case 2:
                MidpointLineForZone0ButModified(2, y1, -x1, y2, -x2);
                break;
            case 3:
                MidpointLineForZone0ButModified(3, -x1, y1, -x2, y2);
                break;
            case 4:
                MidpointLineForZone0ButModified(4, -x1, -y1, -x2, -y2);
                break;
            case 5:
                MidpointLineForZone0ButModified(5, -y1, -x1, -y2, -x2);
                break;
            case 6:
                MidpointLineForZone0ButModified(6, -y1, x1, -y2, x2);
                break;
            case 7:
                MidpointLineForZone0ButModified(7, x1, -y1, x2, -y2);
                break;
        }
    }
    private void MidpointLineForZone0ButModified(int zone, double x1, double y1, double x2, double y2) {
        //System.out.println("encoded zone 0 points : ("+x1+","+y1+"), ("+x2+","+y2+")");
        double dx = x2-x1;
        double dy = y2-y1;
        double d= 2*dy-dx;
        double dE = 2*dy;
        double dNE = 2*(dy-dx);

        double x=x1, y=y1;

        gl.glBegin(GL2.GL_POINTS);

        while(x<=x2){
            switch (zone){
                //decoding the zones from zone 0
                case 0:
                    gl.glVertex2d(x, y);
                    break;
                case 1:
                    gl.glVertex2d(y, x);
                    break;
                case 2:
                    gl.glVertex2d(-y, x);
                    break;
                case 3:
                    gl.glVertex2d(-x, y);
                    break;
                case 4:
                    gl.glVertex2d(-x, -y);
                    break;
                case 5:
                    gl.glVertex2d(-y, -x);
                    break;
                case 6:
                    gl.glVertex2d(y, -x);
                    break;
                case 7:
                    gl.glVertex2d(x, -y);
                    break;
            }

            if(d<=0){
                d = d + dE;
            }
            else{
                y++;
                d = d + dNE;
            }
            x++;
        }

        gl.glEnd();
    }
    private int getZone(double x1, double y1, double x2, double y2){
        double dx = x2-x1;
        double dy = y2-y1;
        int zone=-1;

        //1st quadrant
        if (dx>0 && dy>0 && Math.abs(dx)>Math.abs(dy)){zone = 0;}
        if (dx>0 && dy>0 && Math.abs(dy)>Math.abs(dx)){zone = 1;}
        //2nd quadrant
        if (dx<0 && dy>0 && Math.abs(dy)>Math.abs(dx)){zone = 2;}
        if (dx<0 && dy>0 && Math.abs(dx)>Math.abs(dy)){zone = 3;}
        //3rd quadrant
        if (dx<0 && dy<0 && Math.abs(dx)>Math.abs(dy)){zone = 4;}
        if (dx<0 && dy<0 && Math.abs(dy)>Math.abs(dx)){zone = 5;}
        //4th quadrant
        if (dx>0 && dy<0 && Math.abs(dy)>Math.abs(dx)){zone = 6;}
        if (dx>0 && dy<0 && Math.abs(dx)>Math.abs(dy)){zone = 7;}

        //System.out.println("zone = "+zone);
        return zone;
    }

    //MidpointLineDrawing_Algo for zone = 0
    private void MidpointLineForZone0(GL2 gl, double x1, double y1, double x2, double y2){
        double dx = x2-x1;
        double dy = y2-y1;
        double d= 2*dy-dx;
        double dE = 2*dy;
        double dNE = 2*(dy-dx);

        double x=x1, y=y1;

        gl.glBegin(GL2.GL_POINTS);

        while(x<=x2){

            gl.glVertex2d(x, y);

            if(d<=0){
                d = d + dE;
            }
            else{
                y++;
                d = d + dNE;
            }
            x++;
        }

        gl.glEnd();
    }

    //BruteForce_Algo
    private void BruteForceDrawLine(GL2 gl, double x1, double y1, double x2, double y2){
        double m = (y2 - y1) / (x2 - x1); //slope calc
        double c = y1 - m*x1;//intercept calc

        gl.glBegin(GL2.GL_POINTS);

        if(m<=1){
            for(double x=x1; x<=x2; x++){
                double y = m*x + c;
                gl.glVertex2d(x, y);
            }
        }
        else{
            for(double y=y1; y<=y2; y++){
                double x = (y - c)/ m ;
                gl.glVertex2d(x, y);
            }
        }
        gl.glEnd();
    }

    //DDA_Algo
    private void DDA_Algo(GL2 gl, double x1, double y1, double x2, double y2){
        double m = (y2 - y1) / (x2 - x1); //slope calc

        gl.glBegin(GL2.GL_POINTS);
        gl.glColor3d(0.0, 1.0, 0);
        
        double x=x1, y=y1;
        if(m<=1){
            while(x<=x2){
                x++; y+=m;
                gl.glVertex2d(x, y);
            }
        }
        else{
            while(y<=y2){
                y++; x+=1/m;
                gl.glVertex2d(x, y);
            }
        }
        gl.glEnd();
    }

    /**
     * Take care of initialization here.
     */
    @Override
    public void init(GLAutoDrawable gld) {
        GL2 gl = gld.getGL().getGL2();
        glu = new GLU();

        gl.glClearColor(1.0f, 1.0f, 1.0f, 1.0f);
        gl.glViewport(-250, -150, 250, 150);
        gl.glMatrixMode(GL2.GL_PROJECTION);
        gl.glLoadIdentity();
        //glu.gluOrtho2D(-350.0, 350.0, -250.0, 250.0);
        glu.gluOrtho2D(-800.0, 800.0, -500.0, 500.0);
    }

    /**
     * Take care of drawing here.
     */
    @Override
    public void display(GLAutoDrawable drawable) {
        gl = drawable.getGL().getGL2();

        gl.glClear(GL2.GL_COLOR_BUFFER_BIT);
        /*
         * put your code here
         */
        doIt(gl);
    }

    @Override
    public void reshape(GLAutoDrawable drawable, int x, int y, int width, int height) {
    }
    public void displayChanged(GLAutoDrawable drawable, boolean modeChanged, boolean deviceChanged) {
    }
    @Override
    public void dispose(GLAutoDrawable arg0) { }

} // 'ThirdGLEventListener' class

public class JavaApplication1
{
    public static void main(String args[])
    {
        //getting the capabilities object of GL2 profile
        final GLProfile profile=GLProfile.get(GLProfile.GL2);
        GLCapabilities capabilities=new GLCapabilities(profile);
        // The canvas
        final GLCanvas glcanvas=new GLCanvas(capabilities);
        ThirdGLEventListener b=new ThirdGLEventListener();
        glcanvas.addGLEventListener(b);
        glcanvas.setSize(900, 600);
        //creating frame
        final JFrame frame=new MyJFrame();
        frame.setTitle("LifeView frame");
        //adding canvas to frame
        frame.add(glcanvas);
        frame.setSize(900,600);
        frame.setVisible(true);
    }

    static class MyJFrame extends JFrame {
        protected void processWindowEvent(WindowEvent e) {
            if (e.getID() == WindowEvent.WINDOW_CLOSING) {
                System.exit(0);
            }
        }
    }
}// 'Main' class
