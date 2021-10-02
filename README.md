using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Что_то_про_графики
{
    public partial class Form1 : Form
    {
        Pen blackpen;
        bool symbol=false;
        Random rnd=new Random();
        int count;
        public Form1()
        {
            InitializeComponent();
            pictureBox1.Location = new Point(199,99);
            pictureBox1.Width = 1701;
            pictureBox1.Height = 701;
            textBox1.Height = 300;
            button2.Location = new Point(textBox1.Location.X, textBox1.Location.Y + 320);
            button4.Location = new Point(button2.Location.X, button2.Location.Y + 100);
            button3.Location = new Point(button2.Location.X, button2.Location.Y + 200);
            blackpen = new Pen(Color.MidnightBlue, 5);
            blackpen.StartCap = System.Drawing.Drawing2D.LineCap.Round;
            blackpen.EndCap = System.Drawing.Drawing2D.LineCap.Round;
            button1.Location = new Point(200,pictureBox1.Height + 145);
            label2.Location = new Point(370, pictureBox1.Height + 150);
            label4.Location = new Point(510, pictureBox1.Height + 150);
            label5.Location = new Point(640, pictureBox1.Height + 150);
            label6.Location = new Point(330, pictureBox1.Height + 190);
            label7.Location = new Point(330, pictureBox1.Height + 220);
            button5.Location = new Point(1800, 900);
        }

        private class ArrayChart
        {
            private int index;
            private double[] pointX;
            private double[] pointY;
            public ArrayChart(int size)
            {
                pointX = new double[size];
                pointY = new double[size];
            }
            public void SetPoint(double x, double y)
            {
                pointX[index] = x;
                pointY[index] = y;
                index++;
            }
            public void ResetPoints()
            {
                index = 0;
                Array.Clear(pointX, 0, pointX.Length);
                Array.Clear(pointY, 0, pointY.Length);
            }
            public double GetPointX(int i)
            {
                return pointX[i];
            }
            public double GetPointY(int i)
            {
                return pointY[i];
            }
           
            public int ReturnPoint(int x, int y)
            {
                int p = -1;
                for (int i = 0; i < index; i++)
                {
                    if (x == pointX[i] && y == pointY[i])
                    {
                        p = i;
                        break;
                    }
                    else p = -1;
                }
                return p;
            }
            public int CheckLocation(int x,int y)
            {
                int k=-1, rx,rx1,ry,ry1;
                rx = x + 7; rx1 = x-7;
                ry = y + 7; ry1 = y - 7;
                for (int i=0; i<index; i++)
                {
                    if (x == pointX[i] && y == pointY[i] || (rx>pointX[i]&&rx1<pointX[i]&&ry>pointY[i]&&ry1<pointY[i]))
                    {
                         k = i;
                        break;
                    }
                    else k = -1;
                }
                return k;
            }
        }
        private ArrayChart arrayPoints = new ArrayChart(51);

        private void button1_Click(object sender, EventArgs e)
        {
            double max_x = arrayPoints.GetPointX(0), max_y = arrayPoints.GetPointY(0), average_value_x=0, average_value_y=0;
            for (int i=1; i<count; i++)
            {
                if (max_x < arrayPoints.GetPointX(i))
                {
                    max_x = arrayPoints.GetPointX(i);
                }
                if (max_y >  arrayPoints.GetPointY(i))
                {
                    max_y =  arrayPoints.GetPointY(i);
                }
                average_value_x += arrayPoints.GetPointX(i);
                average_value_y += arrayPoints.GetPointY(i);
            }
            double min_x = arrayPoints.GetPointX(0), min_y = arrayPoints.GetPointY(0);
            for (int i = 1; i < count; i++)
            {
                if (min_x > arrayPoints.GetPointX(i))
                {
                    min_x = arrayPoints.GetPointX(i);
                }
                if (min_y < arrayPoints.GetPointY(i))
                {
                    min_y = arrayPoints.GetPointY(i);
                }
            }

            label8.Text = Convert.ToString((max_x - pictureBox1.Width / 2) / 8.5); label9.Text = Convert.ToString((max_y - pictureBox1.Height / 2) / (-3.5));
            label8.Location = new Point(420, pictureBox1.Height + 190); label8.Visible = true;
            label9.Location = new Point(420, pictureBox1.Height + 220); label9.Visible = true;

            label10.Text = Convert.ToString((min_x - pictureBox1.Width / 2) / 8.5); label11.Text = Convert.ToString((min_y - pictureBox1.Height / 2) / (-3.5));
            label10.Location = new Point(540, pictureBox1.Height + 190); label10.Visible = true;
            label11.Location = new Point(540, pictureBox1.Height + 220); label11.Visible = true;

            average_value_x= (average_value_x / count - pictureBox1.Width / 2) / 8.5;
            average_value_y= (average_value_y / count - pictureBox1.Height / 2) / (-3.5);
            label12.Text = Convert.ToString(Math.Round(average_value_x,3)); label13.Text = Convert.ToString(Math.Round(average_value_y, 3));
            label12.Location = new Point(690, pictureBox1.Height + 190); label12.Visible = true;
            label13.Location = new Point(690, pictureBox1.Height + 220); label13.Visible = true;
        }
        
        Bitmap map = new Bitmap(1700, 700);
        private void SetPicture()
        {
            map = new Bitmap(1700, 700);
            g = Graphics.FromImage(map);
        }
        private void button2_Click(object sender, EventArgs e)
        {
            button1.Enabled = true;
            button4.Enabled = true;
            symbol = true;
            ResetGraph();
            int[] a = new int[51];
            int[] b = new int[51];
            textBox1.Text = "#   X     Y";
            int  x,y;
             count = rnd.Next(2, 50);
            for (int i=0; i<count; i++)
            {
                x = rnd.Next(-100,100);
                y = rnd.Next(-100, 100);
                if (x > 0 && y>0)
                {
                    textBox1.Text = textBox1.Text + Environment.NewLine + (i + 1) + ".    " + x + "    " + y;
                }
                else if (x>0 && y < 0)
                {
                    textBox1.Text = textBox1.Text + Environment.NewLine + (i + 1) + ".    " + x + "   " + y;
                }
                else if (x < 0 && y > 0)
                {
                    textBox1.Text = textBox1.Text + Environment.NewLine + (i + 1) + ".   " + x + "    " + y;
                }
                else { textBox1.Text = textBox1.Text + Environment.NewLine + (i + 1) + ".   " + x + "   " + y; }
                a[i] = x; b[i] = y;
               
                
            }
            //sort
            for (int j=0; j<count-1; j++)
            {
                for (int i=1; i<count; i++)
                {
                    if (a[i - 1]< a[i])
                    {
                        int t;
                        t = a[i];
                        a[i] = a[i - 1];
                        a[i - 1] = t;
                        t = b[i];
                        b[i] = b[i - 1];
                        b[i - 1] = t;
                    }
                }
            }
            for (int i = 0; i < count; i++)
            {
                
                arrayPoints.SetPoint(a[i]*8.5+pictureBox1.Width/2 , (-1*b[i]*3.5)+pictureBox1.Height/2);
            }
           
   
        }

        private void textBox1_TextChanged(object sender, EventArgs e)
        {

        }
        Graphics g;
        private void AXIS()
        {
            blackpen.Width = 4;
            g.DrawLine(blackpen, 0, pictureBox1.Height / 2, pictureBox1.Width, pictureBox1.Height / 2);
            g.DrawLine(blackpen, pictureBox1.Width / 2, 0, pictureBox1.Width / 2, pictureBox1.Height);
            Font myfont = new Font("Arial", 10);
            g.DrawString("0", myfont, Brushes.Black, pictureBox1.Width/2-20, pictureBox1.Height/2+15);
            g.DrawLine(blackpen, pictureBox1.Width / 2, 0, pictureBox1.Width / 2 - 10, 0 + 30);
            g.DrawLine(blackpen, pictureBox1.Width / 2, 0, pictureBox1.Width / 2 + 10, 0 + 30);
            g.DrawString("y", new Font("Arial", 12), Brushes.Black, pictureBox1.Width / 2 + 20, 0 );
            g.DrawLine(blackpen, pictureBox1.Width, pictureBox1.Height/2, pictureBox1.Width - 30, pictureBox1.Height/2 + 10);
            g.DrawLine(blackpen, pictureBox1.Width, pictureBox1.Height / 2, pictureBox1.Width - 30, pictureBox1.Height / 2 - 10);
            g.DrawString("x", new Font("Arial", 12), Brushes.Black, pictureBox1.Width - 30, pictureBox1.Height / 2 + 15);
            //X
            int k = 0;
            for (int i = pictureBox1.Width / 2 - 85; i > 0; i -= 85)
            {
                g.DrawLine(blackpen, i, pictureBox1.Height / 2 + 10, i, pictureBox1.Height / 2 - 10);
                k = k - 10;
                g.DrawString(k.ToString(), myfont, Brushes.Black, i - 10, pictureBox1.Height / 2 + 15);
            }
            k = 0;
            for (int i = pictureBox1.Width / 2 + 85; i <pictureBox1.Width-85; i += 85)
            {
                g.DrawLine(blackpen, i, pictureBox1.Height / 2 + 10, i, pictureBox1.Height / 2 - 10);
                k = k + 10;
                g.DrawString(k.ToString(), myfont, Brushes.Black, i - 10, pictureBox1.Height / 2 + 15);
            }
            //Y
            k = 0;
            for (int i = pictureBox1.Height / 2 - 35; i > 0; i -= 35)
            {
                g.DrawLine(blackpen, pictureBox1.Width/2-10, i, pictureBox1.Width / 2 + 10, i);
                k = k + 10;
                g.DrawString(k.ToString(), myfont, Brushes.Black, pictureBox1.Width/2+10, i-10);
            }
            k = 0;
            for (int i = pictureBox1.Height / 2 + 35; i < pictureBox1.Height-35; i += 35)
            {
                g.DrawLine(blackpen, pictureBox1.Width / 2 - 10, i, pictureBox1.Width / 2 + 10, i);
                k = k - 10;
                g.DrawString(k.ToString(), myfont, Brushes.Black, pictureBox1.Width / 2 + 10, i - 10);
            }
        }
        
        private void button4_Click(object sender, EventArgs e)
        {
            g = Graphics.FromImage(map);
            button3.Enabled = true;
            AXIS();
            Pen graphPen = new Pen(Color.HotPink, 4);
            graphPen.StartCap = System.Drawing.Drawing2D.LineCap.Round;
            graphPen.EndCap = System.Drawing.Drawing2D.LineCap.Round;
            graphPen.StartCap = System.Drawing.Drawing2D.LineCap.Flat;
            graphPen.EndCap = System.Drawing.Drawing2D.LineCap.Flat;
            for (int i = 0; i < count-1; i++)
            {
                g.DrawLine(graphPen, Convert.ToInt32(arrayPoints.GetPointX(i)), Convert.ToInt32(arrayPoints.GetPointY(i)), Convert.ToInt32(arrayPoints.GetPointX(i+1)), Convert.ToInt32(arrayPoints.GetPointY(i+1)));
                g.FillEllipse(Brushes.Indigo , Convert.ToInt32(arrayPoints.GetPointX(i) - 7), Convert.ToInt32(arrayPoints.GetPointY(i) - 7), 15, 15);
               
                pictureBox1.Image = map;
            }
            g.FillEllipse(Brushes.Indigo, Convert.ToInt32(arrayPoints.GetPointX(count-1) - 7), Convert.ToInt32(arrayPoints.GetPointY(count-1) - 7), 15,15);
        }

        private void ResetGraph()
        {
            pictureBox1.Image = null;
            map = null;
            SetPicture();
            arrayPoints.ResetPoints();
            textBox1.Text = null;
            //textBox2.Text = null;
            AXIS();
            pictureBox1.Image = map;
        }
        private void button3_Click(object sender, EventArgs e)
        {
            ResetGraph();
            button3.Enabled = false;
            button1.Enabled = false;
            label8.Text = null;
            label9.Text = null;
            label10.Text = null;
            label11.Text = null;
            label12.Text = null;
            label13.Text = null;
        }

        private void pictureBox1_MouseMove(object sender, MouseEventArgs e)
        {
            int k = arrayPoints.CheckLocation(e.X, e.Y);
            if (k!=(-1))
            {
                if (e.X > 1600)
                {
                    label1.Location = new Point(e.X+100, e.Y + 90);
                }
                else
                {
                    label1.Location = new Point(e.X + 220, e.Y + 90);
                }
                label1.Visible = true;
                double x, y;
                x = arrayPoints.GetPointX(k);
                y = arrayPoints.GetPointY(k);
                label1.Text = "X:" + (x-pictureBox1.Width/2)/8.5 + " Y:" + (y-pictureBox1.Height/2)/(-3.5);
                label1.BackColor = pictureBox1.BackColor;

            }
            else
            {
                label1.Visible = false;
            }
        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }

        private void button5_Click(object sender, EventArgs e)
        {
            Application.Exit();
        }
    }
}
