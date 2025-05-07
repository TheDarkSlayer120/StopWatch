import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class Stopwatch extends JFrame implements ActionListener {

    private JLabel timeLabel;
    private JButton startButton, stopButton, resetButton;

    private Timer timer;
    private long startTime;
    private boolean running;

    public Stopwatch() {
        // Window setup
        setTitle("Stopwatch");
        setSize(300, 200);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(new BorderLayout());
        setIconImage(logo.getImage());

        // Time display
        timeLabel = new JLabel("00:00:00", SwingConstants.CENTER);
        timeLabel.setFont(new Font("Arial", Font.BOLD, 40));
        add(timeLabel, BorderLayout.CENTER);

        // Buttons
        JPanel buttonPanel = new JPanel();
        startButton = new JButton("Start");
        stopButton = new JButton("Stop");
        resetButton = new JButton("Reset");

        startButton.addActionListener(this);
        stopButton.addActionListener(this);
        resetButton.addActionListener(this);

        buttonPanel.add(startButton);
        buttonPanel.add(stopButton);
        buttonPanel.add(resetButton);
        add(buttonPanel, BorderLayout.SOUTH);

        // Timer setup
        timer = new Timer(1000, e -> updateDisplay());
        running = false;

        // Make it visible
        setLocationRelativeTo(null); // Center on screen
        setVisible(true);
    }

    private void updateDisplay() {
        long elapsed = (System.currentTimeMillis() - startTime) / 1000;
        int hours = (int) (elapsed / 3600);
        int minutes = (int) ((elapsed % 3600) / 60);
        int seconds = (int) (elapsed % 60);

        String timeText = String.format("%02d:%02d:%02d", hours, minutes, seconds);
        timeLabel.setText(timeText);
    }

    public void actionPerformed(ActionEvent e) {
        Object source = e.getSource();

        if (source == startButton && !running) {
            startTime = System.currentTimeMillis() - getElapsedMillis();
            timer.start();
            running = true;
        } else if (source == stopButton && running) {
            timer.stop();
            running = false;
        } else if (source == resetButton) {
            timer.stop();
            running = false;
            startTime = 0;
            timeLabel.setText("00:00:00");
        }
    }

    private long getElapsedMillis() {
        String[] parts = timeLabel.getText().split(":");
        int hrs = Integer.parseInt(parts[0]);
        int mins = Integer.parseInt(parts[1]);
        int secs = Integer.parseInt(parts[2]);
        return (hrs * 3600 + mins * 60 + secs) * 1000L;
    }
    
    //ImageIcon logo = new ImageIcon(".//res//Stopwatch.png");
    ImageIcon logo = new ImageIcon(getClass().getClassLoader().getResource("Stopwatch.png"));
    
    

    // ✅ This is the correct entry point
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new Stopwatch());
    }
}
