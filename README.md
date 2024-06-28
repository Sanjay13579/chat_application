Server Side
package applet.text;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.*;
import java.net.*;

public class Nivi extends Frame implements Runnable,ActionListener {
	 Button button = new Button("Click it");
     TextArea txt = new TextArea();
     TextField tf = new TextField();
     DataInputStream dti;
     DataOutputStream doi;
    
    public Nivi() {
        add(tf);
        add(txt);
        add(button);
        setTitle("Nivi");

        tf.setBounds(50, 50, 400, 50);  // x, y, width, height

        txt.setBounds(50, 170, 400, 200); // example positioning

        button.setBounds(200, 400, 80, 30);
        button.addActionListener(this);
        setSize(500, 500);

        addWindowListener(new java.awt.event.WindowAdapter() {
            public void windowClosing(java.awt.event.WindowEvent windowEvent) {
                System.exit(0);
            }
        });

        setLayout(null);

        setVisible(true);
        try {
        	ServerSocket ssoc= new ServerSocket(12000);
        	Socket soc = ssoc.accept();
        	dti = new DataInputStream(soc.getInputStream());
        	doi = new DataOutputStream(soc.getOutputStream());
        }
        catch(Exception e) {}
        Thread t = new Thread(this);
   	 t.setDaemon(true);
        t.start();
        
    }
    public static void main(String[] args) {
        Nivi nivi = new Nivi();
    }
    public void run(){
        while(true)
			try {
				String msg = dti.readUTF();
				txt.append("Sanjay: "+ msg+"\n");
			} catch (IOException e) {}
        }
	@Override
	public void actionPerformed(ActionEvent e) {
		// TODO Auto-generated method stub
		String msg = tf.getText();
		txt.append("Me: " + msg+"\n");
		tf.setText("");
		try {
			doi.writeUTF(msg);
			doi.flush();
		} catch (IOException e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		}
	}
}
client side
package applet.text;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.*;

public class Sanjay extends Frame implements Runnable,ActionListener{
	Button button = new Button("Click it");
    TextArea txt = new TextArea();
    TextField tf = new TextField();
    DataInputStream dti;
    DataOutputStream doi;
   
   public Sanjay() {

        
       add(tf);
       add(txt);
       add(button);
       setTitle("Sanjay");
       tf.setBounds(50, 50, 400, 50);  // x, y, width, height

       txt.setBounds(50, 170, 400, 200); // example positioning

       button.setBounds(200, 400, 80, 30);
       button.addActionListener(this);
       setSize(500, 500);

       addWindowListener(new java.awt.event.WindowAdapter() {
           public void windowClosing(java.awt.event.WindowEvent windowEvent) {
               System.exit(0);
           }
       });

       setLayout(null);

       setVisible(true);
       try {
       	
       	Socket soc = new Socket("localhost",12000);
       	dti = new DataInputStream(soc.getInputStream());
       	doi = new DataOutputStream(soc.getOutputStream());
       }
       catch(Exception e) {}
     	 Thread t = new Thread(this);
        	t.setDaemon(true);
             t.start();
       
   }
   public static void main(String[] args) {
      new Sanjay();
   }
   public void run(){
       while(true)
			try {
				String msg = dti.readUTF();
				txt.append("Nivi: "+msg+"\n");
			} catch (IOException e) {}
       }
	@Override
	public void actionPerformed(ActionEvent e) {
		// TODO Auto-generated method stub
		String msg = tf.getText();
		txt.append("Me: " + msg+"\n");
		tf.setText("");
		try {
			doi.writeUTF(msg);
			doi.flush();
		} catch (IOException e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		}
	}
}
