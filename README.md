# panel.java
import java.awt.Color;
import java.awt.Dimension;
import java.awt.Graphics;
import java.awt.Image;
 
import javax.swing.JPanel;
 
public class Panel extends JPanel implements Runnable {
     
    private Image dbImage;
    private Graphics dbGraphics;
    int GWidth = 500;
    int GHeight = 400;
    Dimension dim = new Dimension(GWidth, GHeight);
    private Thread game;
    private volatile boolean running = false;
    Tile tile;
 
    public Panel(){
         
        setPreferredSize(dim);
        setBackground(Color.black);
        setFocusable(true); // Focus on JPanel to receive key events
        requestFocus();
 
        tile = new Tile();
 
    }
 
    public void draw(Graphics g) {
        tile.draw(g);
 
    }
 
    public void run() {
        while (running) {
            update();
            render();
            paintScreen();
 
        }
    }
 
    private void startGame() {
        if (game == null || !running) {
            game = new Thread(this);
            game.start();
            running = true;
        }
 
    }
 
    private void update() {
        if (game != null && running == true) {
 
        }
    }
 
    private void render() {
        if (dbImage == null) {
            dbImage = createImage(GWidth, GHeight);
 
        if (dbImage == null) {
            System.err.println("error");
            return;
            } else {
            dbGraphics = dbImage.getGraphics();
            }
        }
         
        dbGraphics.setColor(Color.WHITE);
        dbGraphics.fillRect(0, 0, GWidth, GHeight);
        draw(dbGraphics);
    }
 
    private void paintScreen() {
        Graphics g;
        try {
            g = this.getGraphics();
            if (dbImage != null && g != null) {
                g.drawImage(dbImage, 0, 0, null);
            }
 
        } catch (Exception e) {
            System.err.println(e);
        }
    }
 
    public void stopGame() {
        if (running) {
            running = false;
        }
    }
 
    public void addNotify() {
        super.addNotify();
        startGame();
 
    }
 
 
}
